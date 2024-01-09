112-1 Logical design final project

作者:111321025 , 111321027 , 111321031

藍色燈代表：角色

紅色燈代表：掉落物

綠色燈代表：射擊子彈

input/output unit
七段顯示器：時間計時、
8*8LED顯示器：遊戲畫面、
bottum：控制左右移動,射擊,reset

起始畫面 image
![DSC_0283](https://github.com/FrankChen0930/FrankChen/assets/113695822/932db72c-b68c-469e-863c-822f973acac6)


掉落物掉落 隨機掉落 可以多排同時掉落 image
![DSC_0284](https://github.com/FrankChen0930/FrankChen/assets/113695822/6406badb-5ccd-4483-928a-22c5a0f687a5)


射擊畫面 可以無限發射 image
![DSC_0285](https://github.com/FrankChen0930/FrankChen/assets/113695822/df939e72-b84f-4fc5-9ec2-95a7bfd90914)


失敗畫面 血條有5個 image
![DSC_0282](https://github.com/FrankChen0930/FrankChen/assets/113695822/c8ee3b6e-06e2-47be-be16-e1bf159bcdcf)


通關畫面 時間到60秒 通關 image
![DSC_0286](https://github.com/FrankChen0930/FrankChen/assets/113695822/31a333ae-633b-4502-af62-fcaa07113d8c)



功能說明:

持續躲避和射擊掉落物直到時間到為止（掉落物可能是一格或是橫排2*直排1）

module test(output reg [7:0] DATA_R, DATA_G, DATA_B, //8*8LED輸出(RED,GREEN,BLUE)

output reg [3:0] COMM, //8*8LED輸出控制

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

reg [1:0] count_ab; //ab排同時掉落

reg [1:0] count_cd;//cd排同時掉落

reg [1:0] count_ef;//ef排同時掉落

reg [1:0] count_gh;//gh排同時掉落

實作影片上傳在上面的檔案中喔

或是參考下面的連結
https://drive.google.com/drive/folders/1g8JTZDdyW9PUaCGHYFX59dpitA-sUuD_?usp=sharing
