# logic_final
109 Logical design final project <BR>
 
作者:108321018 , 108321013 <BR>

藍色代表 操作腳色 <BR>
紅色代表 掉落物   <BR>
綠色代表 射擊子彈 <BR>

input/output unit  <BR>
七段顯示器:時間計時  <BR>
8*8LED顯示器:遊戲畫面 <BR>
bottum:控制左右移動,射擊,reset <BR>

起始畫面
![image](https://github.com/lemontea8909/logic_final/blob/main/DSC_0283.JPG)

掉落物掉落 隨機掉落 可以多排同時掉落
![image](https://github.com/lemontea8909/logic_final/blob/main/DSC_0284.JPG)

射擊畫面 可以無限發射
![image](https://github.com/lemontea8909/logic_final/blob/main/DSC_0285.JPG)

失敗畫面 血條有5個
![image](https://github.com/lemontea8909/logic_final/blob/main/DSC_0282.JPG)

通關畫面  時間到60秒 通關
![image](https://github.com/lemontea8909/logic_final/blob/main/DSC_0286.JPG)

功能說明:   <BR>
持續躲避和射擊掉落物直到時間到為止~<BR>

module test(output reg [7:0] DATA_R, DATA_G, DATA_B,   //8*8LED輸出(RED,GREEN,BLUE)   <BR>
            output reg [3:0] COMM,                     //8*8LED輸出控制                <BR>
				        output reg [6:0] seg,                      //七段顯示器輸出(a,b,c,d,e,f,g)  <BR>
			         output reg [3:0] seg_cnt,                  //七段顯示器輸出控制             <BR>
            input left,right,reset,shoot,              //左移,右移,重置,射擊 按鍵  接到4-bit SW  <BR>
            input CLK);                                //使用內建CLK PIN_22            <BR>

reg [2:0] position;    //character's position  <BR>
bit [3:0] a,b,c,d,e,f,g,h;   //a到h行的掉落物counter 初始值為8 當觸發啟動a排的掉落物的時候 將a <= 0 由0開始+1到8 <BR>
bit [7:0] a_move [7:0];      //用於儲存目前a排的掉落物的矩陣 <BR>
bit [7:0] b_move [7:0];<BR>
bit [7:0] c_move [7:0];<BR>
bit [7:0] d_move [7:0];<BR>
bit [7:0] e_move [7:0];<BR>
bit [7:0] f_move [7:0];<BR>
bit [7:0] g_move [7:0];<BR>
bit [7:0] h_move [7:0];<BR>
reg [7:0] count;  //沒有用到  原本是作為掉落物的測試使用<BR>
bit [2:0] cnt;    //畫面更新的控制<BR>
reg [2:0] gameover;  //遊戲結束<BR>
reg [11:0] life;   //生命值<BR>
bit [7:0] rnd,count_rand;  //產生亂數使用 亂數的結果為rnd <BR>
bit [3:0] a_shoot,b_shoot,c_shoot,d_shoot,e_shoot,f_shoot,g_shoot,h_shoot; //a到h行的射擊子彈counter 初始值為0 當觸發啟動a排的時候 將a_shoot <= 7 由7開始-1到0 <BR>
bit [7:0] a_s_move [7:0];  //用於儲存目前a排的子彈位置的矩陣<BR>
bit [7:0] b_s_move [7:0];<BR>
bit [7:0] c_s_move [7:0];<BR>
bit [7:0] d_s_move [7:0];<BR>
bit [7:0] e_s_move [7:0];<BR>
bit [7:0] f_s_move [7:0];<BR>
bit [7:0] g_s_move [7:0];<BR>
bit [7:0] h_s_move [7:0];<BR>
bit [3:0] s_index;       //用於偵測shoot輸入的index 用behavior model找出要在哪一行射出子彈<BR>
reg       a_reset,b_reset,c_reset,d_reset,e_reset,f_reset,g_reset,h_reset;   //collision 觸發時使用  reset collision triggr 的時候將 a,a_shoot分別設為8,0<BR>
//counter<BR>
reg [3:0] index [3:0];<BR>   
reg [3:0] A_count;    BR>
reg [3:0] C1_count;   //沒有用到<BR>
reg [3:0] C2_count;   //時間計時 百位數<BR>
reg [3:0] C3_count;   //時間計時 十位數<BR>
reg [3:0] C4_count;   //時間計時 個位數<BR>
reg [2:0] seg_count;  

實作影片

https://drive.google.com/file/d/14MKm177YqhXPsRgxx-McmmkNuC0VAwJ1/view?usp=sharing

https://drive.google.com/file/d/14FLKN36rymiORBHcyKPOov4e6K-OfTMX/view?usp=sharing
