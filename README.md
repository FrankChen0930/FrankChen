112-1 Logical design final project

作者:111321025 , 111321027 , 111321031

藍色代表 操作角色
紅色代表 掉落物
綠色代表 射擊子彈

input/output unit
七段顯示器:時間計時
8*8LED顯示器:遊戲畫面
bottum:控制左右移動,射擊,reset

起始畫面 image


![Image](https://github.com/users/FrankChen0930/projects/1/assets/113695822/9c359f7d-5ebe-4eac-96b7-8e947e469904)


掉落物掉落 隨機掉落 可以多排同時掉落 image


![Image](https://github.com/users/FrankChen0930/projects/1/assets/113695822/36db1863-7944-4459-9e44-fa6a292f9db2)


射擊畫面 可以無限發射 image


![Image](https://github.com/users/FrankChen0930/projects/1/assets/113695822/61261004-eda2-42fd-84ee-670381c1b605)


失敗畫面 血條有5個 image


![Image](https://github.com/users/FrankChen0930/projects/1/assets/113695822/adde2423-6e37-4133-a17d-fba2ca797a1b)


通關畫面 時間到60秒 通關 image


![Image](https://github.com/users/FrankChen0930/projects/1/assets/113695822/02d18543-c0f5-4494-83bb-ee925545df51)


功能說明:
持續躲避和射擊掉落物直到時間到為止~

module test(output reg [7:0] DATA_R, DATA_G, DATA_B, //88LED輸出(RED,GREEN,BLUE)
output reg [3:0] COMM, //88LED輸出控制
output reg [6:0] seg, //七段顯示器輸出(a,b,c,d,e,f,g)
output reg [3:0] seg_cnt, //七段顯示器輸出控制
input left,right,reset,shoot, //左移,右移,重置,射擊 按鍵 接到4-bit SW
input CLK); //使用內建CLK PIN_22

reg [2:0] position; //character's position
bit [3:0] a,b,c,d,e,f,g,h; //a到h行的掉落物counter 初始值為8 當觸發啟動a排的掉落物的時候 將a <= 0 由0開始+1到8
bit [7:0] a_move [7:0]; //用於儲存目前a排的掉落物的矩陣
bit [7:0] b_move [7:0];
bit [7:0] c_move [7:0];
bit [7:0] d_move [7:0];
bit [7:0] e_move [7:0];
bit [7:0] f_move [7:0];
bit [7:0] g_move [7:0];
bit [7:0] h_move [7:0];
reg [7:0] count; //沒有用到 原本是作為掉落物的測試使用
bit [2:0] cnt; //畫面更新的控制
reg [2:0] gameover; //遊戲結束
reg [11:0] life; //生命值
bit [7:0] rnd,count_rand; //產生亂數使用 亂數的結果為rnd
bit [3:0] a_shoot,b_shoot,c_shoot,d_shoot,e_shoot,f_shoot,g_shoot,h_shoot; //a到h行的射擊子彈counter 初始值為0 當觸發啟動a排的時候 將a_shoot <= 7 由7開始-1到0
bit [7:0] a_s_move [7:0]; //用於儲存目前a排的子彈位置的矩陣
bit [7:0] b_s_move [7:0];
bit [7:0] c_s_move [7:0];
bit [7:0] d_s_move [7:0];
bit [7:0] e_s_move [7:0];
bit [7:0] f_s_move [7:0];
bit [7:0] g_s_move [7:0];
bit [7:0] h_s_move [7:0];
bit [3:0] s_index; //用於偵測shoot輸入的index 用behavior model找出要在哪一行射出子彈
reg a_reset,b_reset,c_reset,d_reset,e_reset,f_reset,g_reset,h_reset; //collision 觸發時使用 reset collision triggr 的時候將 a,a_shoot分別設為8,0
//counter
reg [3:0] index [3:0];

reg [3:0] A_count; BR> reg [3:0] C1_count; //沒有用到
reg [3:0] C2_count; //時間計時 百位數
reg [3:0] C3_count; //時間計時 十位數
reg [3:0] C4_count; //時間計時 個位數
reg [2:0] seg_count;
