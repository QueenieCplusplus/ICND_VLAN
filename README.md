# ICND VLAN
除了路由器能劃分 Segmentation 廣播區域，也能利用交換器或橋接器劃分廣播區域（虛擬的區域網路）。

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




# 交換器、橋接器的廣播區域劃分（亦稱為虛擬區域網路）

虛擬區域網路能使各地使用者無需顧慮地理位址，形成群組，成為同一廣播區域。對於控制廣播訊息的散佈，因而加強安全和頻寬效能。

劃分基礎不因地，可選擇因主機功能、團隊（部門）性質、應用程式目標，每個交換器的通訊埠僅能提供一個 VLAN 使用，同一 VLAN 的連接埠可分享廣播資訊。


# 交換器彼此相連

利用 ISL Trunk (交換器間連線幹道) 

原理為 Application-specific integrated circuit  特殊應用的積體電路所作用。

* tag 加上標註

  能在同一實體線路上實現多工傳送，有低延遲效果。

* encapsulation 資料框封裝

# 交換器設定虛擬區域網路網域、主幹、通訊埠配置群組

利用 VTP 協定

* 設定的指令

   * delete vtp 重設
   
   * vtp domain + < name > + < mode > 為虛擬區域網路的網域命名
   // 三種模式：server、client、transparent。
   // 倘若某台交換器為無管理網域狀態，則直到它收到經過主幹的網域傳播為止。
   
   * show vtp 顯示 vtp 的狀態
   // vtp 主幹協定幫助資訊同步備份至其他交換器中，方便擴展。
   
   * int + <int no.> + trunk on 設定一交換器介面為主幹埠
  
   * show trunk 顯示主幹狀態
   
   * vlan + < vlan# > + name + < name > 定義一虛擬區域網路及其名稱
   
   * show valn 顯示虛擬區域網路的狀態
   
   * int + <int no.> + vlan membership static + < vlan# > 將一交換器介面指定予 VLAN
  
  * show vlan-membership 顯示虛擬區域網路的成員
  
  * show spantree + < vlan# > 
