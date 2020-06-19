# ICND VLAN
除了路由器能劃分 Segmentation 廣播區域，也能利用交換器或橋接器劃分廣播區域（虛擬的區域網路）。

# 交換技術

https://github.com/QueenieCplusplus/ICND_Bridge

原本一台交換機可劃分四個碰撞區、一個廣播區。
一旦使用虛擬區域網路，則使用一台交換器，（藉由通訊埠或是連接埠）可劃分多個廣播區。
且也可利用 Trunk 技術將多台交換機相互依賴變成主幹相連的相依性交換機群組。

# 路由器的廣播區域劃分

https://github.com/QueenieCplusplus/ICND_Internet_Packet#路由設備 詳見功能之2。

路由器不將廣播封包傳送出去，故路由器的每個介面所形成的網段順勢成為一個廣播區域。


                              
                                             LAN (Broadcast/Collision)
                                

                                                      |
                                 
                                                    eth 0/1
                                                    
                      
    LAN (Broadcast/Collision)   - serial 0/12         Ｒ        lo 0/4 -   LAN (Broadcast/Collision)
                      
                      
                                                   eth 0/0
                              
                                                     |
                                 
                                            LAN (Broadcast/Collision)
                                            
                                            
# 交換機劃分的廣播網域

基於封包來自哪個虛擬區域網路，可區分各種資料是否 filter、forward、flood (Broadcast)!

* 作法：

每個連接埠專屬一個虛擬區域網路，抑或是將 vlan 資訊保存在封包中傳送。
                                            
# Frame

LLC sublayer - SNAP (IEEE 802.2)

     +----------+----------+-----------+--------+----------------+-----+
     | Preamble | Dest Add | Sorce Add | Length | Variable (Data)| FCS |
     +----------+----------+-----------+--------+----------------+-----+
     
會被封裝成
   
     +------------+----------------------------+-----+
     | ISL Header | Encapsulate Ethernet Frame | CRC |
     +------------+----------------------------+-----+
     
            |
            |
            |
            V
            
     +----+-----+-----+---+---+------+----+--------+-----+------+----+
     | DA | TYPE| USER| SA|LEN|AAAA03| HSA| VLAN ID| BPDU| INDEX| RES|
     +----+-----+-----+---+---+------+----+--------+-----+------+----+
     
 
# 虛擬區域網路

交換機的埠能配置 vlan，每個 vlan 都有位址學習、傳送、過濾、避免迴圈，而一個 vlan 可能有多個連接埠～ ???

當交換機的某連接埠收到一資料框時，交換機會將此 fame 發送到目的地 (與源頭同樣虛擬區域網域)，不會送到其他 vlan 上，除非 flood...，即便真的產生無法辨識目的地方而氾濫（氾洪）時，也只會廣播至此設備的所有廣播區，此時此設備為一廣播區而非一埠自成廣播區。
     
# 交換器、橋接器的廣播區域劃分（亦稱為虛擬區域網路）

虛擬區域網路能使各地使用者無需顧慮地理位址，形成群組，成為同一廣播區域。對於控制廣播訊息的散佈，因而加強安全和頻寬效能。

劃分基礎不因地，可選擇因主機功能、團隊（部門）性質、應用程式目標，每個交換器的通訊埠僅能提供一個 VLAN 使用，同一 VLAN 的連接埠可分享廣播資訊。

# 單一連線跨越多部交換機

根據 802.1q 協定，使用 trunk 技術，主幹相連，利用每台交換機的某一埠（主幹用埠)，彼此透過乙太網路主幹道相連，這些孔與主幹負載了多個不同 vlan 的 fames，故 trunk port 需要設定在高速乙太網路埠上。



              sw A   ---------------- 802.1q------------------ sw B
                                      payloads
     port    port   port   trunk port -------  trunk port port  port  port 
     
       |      |      |                                      |     |     |             
       |      |      |                                      |     |     |
       |      |      |                                      |     |     |
      vlan 1  |      |                                      |     |     |
            vlan2    |                                      |     |     |
                   vlan3                                  vlan4   |     |
                                                                vlan5   |
                                                                       vlan 6

# 交換器彼此相連

利用 ISL Trunk (交換器間連線幹道) 

原理為 Application-specific integrated circuit  特殊應用的積體電路所作用，適用於交換器間、交換器對路由器、交換器對擁有 ISL 功能網路卡的伺服器。

* tag 加上標註

  能在同一實體線路上實現多工傳送，有低延遲效果。

* encapsulation 資料框封裝

 不會進入 client 端，資料框進入客戶端便除去 ISL 標頭。
 
 
 LLC sublayer - SNAP (IEEE 802.2)

     +----------+----------+-----------+--------+----------------+-----+
     | Preamble | Dest Add | Sorce Add | Length | Variable (Data)| FCS |
     +----------+----------+-----------+--------+----------------+-----+
  
     
 其 ISL 標頭（表頭）
 
     +----+-----+-----+---+---+------+----+--------+-----+------+----+
     | DA | TYPE| USER| SA|LEN|AAAA03| HSA| VLAN ID| BPDU| INDEX| RES|
     +----+-----+-----+---+---+------+----+--------+-----+------+----+
 
 
     /* DA: 多點傳送目的地位址。*/
     
     /* Type: 為封裝資料框的種類，如 Ethernet、Token Ring、FDDI、ATM。*/
     
     /* User: 為 Ethernet 上的優先權。*/
     
     /* SA: 發出資料的交換器的 MAC 位置。*/
     
     /* Length: 不包含上述四欄位的資料框長度 */
     
     /* AAAA03 為標準 SNAP 802.2 LLC 標頭 */
     
     /* HSA: SA 的前三個位元組，為製造商或組織的獨有識別碼。 */
     
     /* Vlan : 15 個位元的 Vlan 識別碼，僅有後面十個位元用於區別 1024 個 VLAN */
     
     /* BPDU : 用以識別該 frame 是否為 Spanning Tree 之 BPDU 。 
             網橋協議數據單元是包含有關生成樹協議的信息幀。
             交換機使用唯一的源MAC地址將BPDU從其原始端口發送到具有目標MAC的多播地址。 */
             
     /* INDEX: 發送埠的辨識。 */
     
     /* RES: 保留欄位。 */
     

# 交換器設定虛擬區域網域、主幹埠、通訊埠配置虛擬區域網域（成員）

利用 Vlan Trunking Protocol 協定

* 設定的指令

       * delete vtp 重設

       * vtp domain + < name > + < mode > 為虛擬區域網路的網域命名
       // 三種模式：server、client、transparent。
       // 倘若某台交換器為無管理網域狀態，則直到它收到經過主幹的網域傳播為止。

       * show vtp 顯示 vtp 的狀態
       // vtp 主幹協定幫助資訊同步備份至其他交換器中，方便擴展。

       * int + <int no.> + trunk on 設定一交換器介面為主幹埠

       * show trunk 顯示主幹狀態
   
主幹埠功能：https://github.com/QueenieCplusplus/ICND_VLAN/blob/master/README.md#單一連線跨越多部交換機
   
     * vlan + < vlan# > + name + < name > 定義一虛擬區域網路及其名稱

     * show vln 顯示虛擬區域網路的狀態

     * int + <int no.> + vlan membership static + < vlan# > 將一交換器介面指定予 VLAN

    * show vlan-membership 顯示虛擬區域網路的成員
     // membership 這些成員就是指交換器上的埠們。

    * show spantree + < vlan# > 
    
 # Ref Doc
 
 https://www.netadmin.com.tw/netadmin/zh-tw/technology/FFD0629A85794E9BAC2A9173D5B96EDC

  
  
  
