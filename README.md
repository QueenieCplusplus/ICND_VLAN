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

# 交換器主要通道

利用 VTP 協定
