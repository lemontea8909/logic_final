module test(output reg [7:0] DATA_R, DATA_G, DATA_B,
            output reg [3:0] COMM,
				output reg [6:0] seg,
				output reg [3:0] seg_cnt,
            input left,right,reset,shoot,
            input CLK);
reg [2:0] position;    //character's position
bit [3:0] a,b,c,d,e,f,g,h;
bit [7:0] a_move [7:0];
bit [7:0] b_move [7:0];
bit [7:0] c_move [7:0];
bit [7:0] d_move [7:0];
bit [7:0] e_move [7:0];
bit [7:0] f_move [7:0];
bit [7:0] g_move [7:0];
bit [7:0] h_move [7:0];
reg [7:0] count;
bit [2:0] cnt;
reg [2:0] gameover;
reg [11:0] life;
bit [7:0] rnd,count_rand;
bit [3:0] a_shoot,b_shoot,c_shoot,d_shoot,e_shoot,f_shoot,g_shoot,h_shoot;
bit [7:0] a_s_move [7:0];
bit [7:0] b_s_move [7:0];
bit [7:0] c_s_move [7:0];
bit [7:0] d_s_move [7:0];
bit [7:0] e_s_move [7:0];
bit [7:0] f_s_move [7:0];
bit [7:0] g_s_move [7:0];
bit [7:0] h_s_move [7:0];
bit [3:0] s_index;
reg       a_reset,b_reset,c_reset,d_reset,e_reset,f_reset,g_reset,h_reset;
//counter
reg [3:0] index [3:0];
reg [3:0] A_count;
reg [3:0] C1_count;
reg [3:0] C2_count;
reg [3:0] C3_count;
reg [3:0] C4_count;
reg [2:0] seg_count;

parameter logic [7:0] X0 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b01111111};
parameter logic [7:0] X1 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b01111111,
			8'b11111111};
parameter logic [7:0] X2 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b01111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] X3 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b01111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] X4 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b01111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] X5 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b01111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] X6 [7:0]='{
         8'b11111111,
			8'b01111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] X7 [7:0]='{
         8'b01111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] a0 [7:0]='{
         8'b11111110,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] a1 [7:0]='{
         8'b11111101,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] a2 [7:0]='{
         8'b11111011,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] a3 [7:0]='{
         8'b11110111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] a4 [7:0]='{
         8'b11101111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] a5 [7:0]='{
         8'b11011111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] a6 [7:0]='{
         8'b10111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] a7 [7:0]='{
         8'b01111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] b0 [7:0]='{
         8'b11111111,
			8'b11111110,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] b1 [7:0]='{
         8'b11111111,
			8'b11111101,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] b2 [7:0]='{
         8'b11111111,
			8'b11111011,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] b3 [7:0]='{
         8'b11111111,
			8'b11110111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] b4 [7:0]='{
         8'b11111111,
			8'b11101111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] b5 [7:0]='{
         8'b11111111,
			8'b11011111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] b6 [7:0]='{
         8'b11111111,
			8'b10111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] b7 [7:0]='{
         8'b11111111,
			8'b01111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] c0 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111110,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] c1 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111101,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] c2 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111011,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] c3 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11110111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] c4 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11101111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] c5 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11011111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] c6 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b10111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] c7 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b01111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] d0 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111110,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] d1 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111101,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] d2 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111011,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] d3 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11110111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] d4 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11101111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] d5 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11011111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] d6 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b10111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] d7 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b01111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] e0 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111110,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] e1 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111101,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] e2 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111011,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] e3 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11110111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] e4 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11101111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] e5 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11011111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] e6 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b10111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] e7 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b01111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] f0 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111110,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] f1 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111101,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] f2 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111011,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] f3 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11110111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] f4 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11101111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] f5 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11011111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] f6 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b10111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] f7 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b01111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] g0 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111110,
			8'b11111111};
parameter logic [7:0] g1 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111101,
			8'b11111111};
parameter logic [7:0] g2 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111011,
			8'b11111111};
parameter logic [7:0] g3 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11110111,
			8'b11111111};
parameter logic [7:0] g4 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11101111,
			8'b11111111};
parameter logic [7:0] g5 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11011111,
			8'b11111111};
parameter logic [7:0] g6 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b10111111,
			8'b11111111};
parameter logic [7:0] g7 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b01111111,
			8'b11111111};
parameter logic [7:0] h0 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111110};
parameter logic [7:0] h1 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111101};
parameter logic [7:0] h2 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111011};
parameter logic [7:0] h3 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11110111};
parameter logic [7:0] h4 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11101111};
parameter logic [7:0] h5 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11011111};
parameter logic [7:0] h6 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b10111111};
parameter logic [7:0] h7 [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b01111111};
parameter logic [7:0] init [7:0]='{
         8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111,
			8'b11111111};
parameter logic [7:0] ez [7:0]='{
         8'b10110011,
			8'b10101011,
			8'b10011011,
			8'b10111011,
			8'b11111111,
			8'b10101011,
			8'b10101011,
			8'b10000011};
initial
   begin
      COMM = 4'b1000;      //initial position at left corner
		position = 0;        //角色移動
		cnt = 0;             //畫面更新
		a = 9;               //a排counter //drop item
		b = 9;               //b排counter //drop item
		c = 9;               //c排counter //drop item
		d = 9;               //d排counter //drop item
		e = 9;               //e排counter //drop item
		f = 9;               //f排counter //drop item
		g = 9;               //g排counter //drop item
		h = 9;               //h排counter //drop item
		DATA_R = 8'b11111111;
		DATA_G = 8'b11111111;
		DATA_B = 8'b11111111;
	   a_move [7:0] = init; //a排畫面更新用 //drop item
      b_move [7:0] = init; //b排畫面更新用 //drop item
		c_move [7:0] = init; //c排畫面更新用 //drop item
		d_move [7:0] = init; //d排畫面更新用 //drop item
		e_move [7:0] = init; //e排畫面更新用 //drop item
		f_move [7:0] = init; //f排畫面更新用 //drop item
		g_move [7:0] = init; //g排畫面更新用 //drop item
		h_move [7:0] = init; //h排畫面更新用 //drop item
		a_shoot = 0;          
		b_shoot = 0;
		c_shoot = 0;
		d_shoot = 0;
		e_shoot = 0;
		f_shoot = 0;
		g_shoot = 0;
		h_shoot = 0;
		a_s_move = init;
		b_s_move = init;
		c_s_move = init;
		d_s_move = init;
		e_s_move = init;
		f_s_move = init;
		g_s_move = init;
		h_s_move = init;
		a_reset = 0;
		b_reset = 0;
		c_reset = 0;
		d_reset = 0;
		e_reset = 0;
		f_reset = 0;
		g_reset = 0;
		h_reset = 0;
		life = 3'b001;
		gameover = 0;
		rnd = 20;
		count_rand = 44;
		s_index = 8;
		seg_cnt = 4'b1110;
		seg_count = 0;
		C1_count = 4'b0000;
		C2_count = 4'b0000;
		C3_count = 4'b0000;
		C4_count = 4'b0000;
   end

divfreq F0(CLK,CLK_div);         //clock of output
divfreq_x F1(CLK,CLK_x);         //clock of charactor
divfreq_1 F2(CLK,CLK_1);         //clock of drop item
divfreq_rand F3(CLK,CLK_rand);   //clock of random
divfreq_shoot F4(CLK,CLK_shoot); //clock of shoot
divfreq_s F5(CLK,CLK_s);         //clock of shoot detect
divfreq_2 F6(CLK,CLK_2);         //clock of counter

always@(posedge CLK_2)       //counter
begin
   if(~reset)
	begin   
   if(C4_count == 9)
	   begin
		   if(C3_count == 9)
			   begin
				   if(C2_count == 1)
					   ;
					else
					   begin
						   C2_count <= C2_count + 1;
							C3_count <= 0;
							C4_count <= 0;
						end
				end
			else
		      begin
			      C3_count <= C3_count + 1;
					C4_count <= 0;
			   end
		end
	else
	   C4_count <= C4_count + 1;
	end
	else
	   begin
		   C1_count <= 4'b0000;
			C2_count <= 4'b0000;
			C3_count <= 4'b0000;
			C4_count <= 4'b0000;
      end
end
always@(posedge CLK_s)      //shoot detect
begin
   if(shoot)
	   begin
			if(position == 0)
				s_index <= 7;
			else if(position == 1)
				s_index <= 6;
			else if(position == 2)
				s_index <= 5;
			else if(position == 3)
				s_index <= 4;
			else if(position == 4)
				s_index <= 3;
			else if(position == 5)
				s_index <= 2;
			else if(position == 6)
				s_index <= 1;
			else if(position == 7)
				s_index <= 0;
			else
				s_index <= 4'b1111;
      end
	else
	   s_index <= 4'b1111;
end
always@(posedge CLK_shoot)  //shoot implement
begin
   case({s_index})
		4'b0000:a_shoot <= 7;
		4'b0001:b_shoot <= 7;
		4'b0010:c_shoot <= 7;
		4'b0011:d_shoot <= 7;
		4'b0100:e_shoot <= 7;
		4'b0101:f_shoot <= 7;
		4'b0110:g_shoot <= 7;
		4'b0111:h_shoot <= 7;
		4'b1000:;
	endcase
	// collision
	if(a_reset == 1 || reset == 1)
	   a_shoot <= 0;
	else if(a_shoot > 0)
	   a_shoot <= a_shoot - 1;
   if(b_reset == 1 || reset == 1)
	   b_shoot <= 0;
	else if(b_shoot > 0)
	   b_shoot <= b_shoot - 1;
	if(c_reset == 1 || reset == 1)
	   c_shoot <= 0;
	else if(c_shoot > 0)
	   c_shoot <= c_shoot - 1;
   if(d_reset == 1 || reset == 1)
	   d_shoot <= 0;
	else if(d_shoot > 0)
	   d_shoot <= d_shoot - 1;
	if(e_reset == 1 || reset == 1)
	   e_shoot <= 0;
	else if(e_shoot > 0)
	   e_shoot <= e_shoot - 1;
   if(f_reset == 1 || reset == 1)
	   f_shoot <= 0;
	else if(f_shoot > 0)
	   f_shoot <= f_shoot - 1;
	if(g_reset == 1 || reset == 1)
	   g_shoot <= 0;
	else if(g_shoot > 0)
	   g_shoot <= g_shoot - 1;
   if(h_reset == 1 || reset == 1)
	   h_shoot <= 0;
	else if(h_shoot > 0)
	   h_shoot <= h_shoot - 1;
	//a shoot
	if(a_shoot == 1)
		a_s_move <= a0;
	else if(a_shoot==2)
		a_s_move <= a1;
	else if(a_shoot==3)
		a_s_move <= a2;
	else if(a_shoot==4)
		a_s_move <= a3;
	else if(a_shoot==5)
		a_s_move <= a4;
	else if(a_shoot==6)
		a_s_move <= a5;
	else if(a_shoot==7)
		a_s_move <= a6;
	else
		a_s_move <= init;
	//b shoot
	if(b_shoot == 1)
		b_s_move <= b0;
	else if(b_shoot==2)
		b_s_move <= b1;
	else if(b_shoot==3)
		b_s_move <= b2;
	else if(b_shoot==4)
		b_s_move <= b3;
	else if(b_shoot==5)
		b_s_move <= b4;
	else if(b_shoot==6)
		b_s_move <= b5;
	else if(b_shoot==7)
		b_s_move <= b6;
	else
		b_s_move <= init;
	//c shoot
	if(c_shoot == 1)
		c_s_move <= c0;
	else if(c_shoot==2)
		c_s_move <= c1;
	else if(c_shoot==3)
		c_s_move <= c2;
	else if(c_shoot==4)
		c_s_move <= c3;
	else if(c_shoot==5)
		c_s_move <= c4;
	else if(c_shoot==6)
		c_s_move <= c5;
	else if(c_shoot==7)
		c_s_move <= c6;
	else
		c_s_move <= init;
	//d shoot
	if(d_shoot == 1)
		d_s_move <= d0;
	else if(d_shoot==2)
		d_s_move <= d1;
	else if(d_shoot==3)
		d_s_move <= d2;
	else if(d_shoot==4)
		d_s_move <= d3;
	else if(d_shoot==5)
		d_s_move <= d4;
	else if(d_shoot==6)
		d_s_move <= d5;
	else if(d_shoot==7)
		d_s_move <= d6;
	else
		d_s_move <= init;
	//e shoot
	if(e_shoot == 1)
		e_s_move <= e0;
	else if(e_shoot==2)
		e_s_move <= e1;
	else if(e_shoot==3)
		e_s_move <= e2;
	else if(e_shoot==4)
		e_s_move <= e3;
	else if(e_shoot==5)
		e_s_move <= e4;
	else if(e_shoot==6)
		e_s_move <= e5;
	else if(e_shoot==7)
		e_s_move <= e6;
	else
		e_s_move <= init;
	//f shoot
	if(f_shoot == 1)
		f_s_move <= f0;
	else if(f_shoot==2)
		f_s_move <= f1;
	else if(f_shoot==3)
		f_s_move <= f2;
	else if(f_shoot==4)
		f_s_move <= f3;
	else if(f_shoot==5)
		f_s_move <= f4;
	else if(f_shoot==6)
		f_s_move <= f5;
	else if(f_shoot==7)
		f_s_move <= f6;
	else
		f_s_move <= init;
	//g shoot
	if(g_shoot == 1)
		g_s_move <= g0;
	else if(g_shoot==2)
		g_s_move <= g1;
	else if(g_shoot==3)
		g_s_move <= g2;
	else if(g_shoot==4)
		g_s_move <= g3;
	else if(g_shoot==5)
		g_s_move <= g4;
	else if(g_shoot==6)
		g_s_move <= g5;
	else if(g_shoot==7)
		g_s_move <= g6;
	else
		g_s_move <= init;
   //h shoot
	if(h_shoot == 1)
		h_s_move <= h0;
	else if(h_shoot==2)
		h_s_move <= h1;
	else if(h_shoot==3)
		h_s_move <= h2;
	else if(h_shoot==4)
		h_s_move <= h3;
	else if(h_shoot==5)
		h_s_move <= h4;
	else if(h_shoot==6)
		h_s_move <= h5;
	else if(h_shoot==7)
		h_s_move <= h6;
	else
		h_s_move <= init;
end
always@(posedge CLK_rand)
begin 
   rnd <= rnd + count_rand;
	count_rand = count_rand + 1;
end
always@(posedge CLK_1)       //掉落物事件
begin
   //
   if(rnd % 13 == 5)
	   d <= 0;
	if(rnd % 23 == 3)
	   c <= 0;
	if(rnd % 11 == 2)
	   f <= 0;
	if(rnd % 19 == 7)
	   g <= 0;
	if(rnd % 9 == 2)
	   a <= 0;
	if(rnd % 31 == 13)
	   b <= 0;
	if(rnd % 8 == 4)
	   h <= 0;
	if(rnd % 8 == 1)
	   e <= 0;
   /////////////////
//	if(rnd % 13 == 5)
//	   d <= 0;
//	if(rnd % 23 == 3)
//	   c <= 0;
//	if(rnd % 11 == 10)
//	   f <= 0;
//	if(rnd % 19 == 7)
//	   g <= 0;
//	if(rnd % 9 == 2)
//	   a <= 0;
//	if(rnd % 31 == 13)
//	   b <= 0;
//	if(rnd % 79 == 34)
//	   h <= 0;
   //collision
	if(a_reset == 1 || reset == 1)
	   a <= 8;
	else if(a < 9)
		a <= a + 1;
	if(b_reset == 1 || reset == 1)
	   b <= 8;
	else if(b < 9)
	   b <= b + 1;
	if(c_reset == 1 || reset == 1)
	   c <= 8;
	else if(c < 9)
	   c <= c + 1;
	if(d_reset == 1 || reset == 1)
	   d <= 8;
	else if(d < 9)
	   d <= d + 1;
	if(e_reset == 1 || reset == 1)
	   e <= 8;
	else if(e < 9)
	   e <= e + 1;
	if(f_reset == 1 || reset == 1)
	   f <= 8;
	else if(f < 9)
	   f <= f + 1;
	if(g_reset == 1 || reset == 1)
	   g <= 8;
	else if(g < 9)
	   g <= g + 1;
	if(h_reset == 1 || reset == 1)
	   h <= 8;
	else if(h < 9)
	   h <= h + 1;
   // a~h drop item count

	if(a == 0)
		a_move <= a0;
	else if(a==1)
		a_move <= a1;
	else if(a==2)
		a_move <= a2;
	else if(a==3)
		a_move <= a3;
	else if(a==4)
		a_move <= a4;
	else if(a==5)
		a_move <= a5;
	else if(a==6)
		a_move <= a6;
	else if(a==7)
		a_move <= a7;
	else
		a_move <= init;

	if(b == 0)
		b_move <= b0;
	else if(b==1)
		b_move <= b1;
	else if(b==2)
		b_move <= b2;
	else if(b==3)
		b_move <= b3;
	else if(b==4)
		b_move <= b4;
	else if(b==5)
		b_move <= b5;
	else if(b==6)
		b_move <= b6;
	else if(b==7)
		b_move <= b7;
	else
		b_move <= init;

	if(c == 0)
		c_move <= c0;
	else if(c==1)
		c_move <= c1;
	else if(c==2)
		c_move <= c2;
	else if(c==3)
		c_move <= c3;
	else if(c==4)
		c_move <= c4;
	else if(c==5)
		c_move <= c5;
	else if(c==6)
		c_move <= c6;
	else if(c==7)
		c_move <= c7;
	else
		c_move <= init;

	if(d == 0)
		d_move <= d0;
	else if(d==1)
		d_move <= d1;
	else if(d==2)
		d_move <= d2;
	else if(d==3)
		d_move <= d3;
	else if(d==4)
		d_move <= d4;
	else if(d==5)
		d_move <= d5;
	else if(d==6)
		d_move <= d6;
	else if(d==7)
		d_move <= d7;
	else
		d_move <= init;

	if(e == 0)
		e_move <= e0;
	else if(e==1)
		e_move <= e1;
	else if(e==2)
		e_move <= e2;
	else if(e==3)
		e_move <= e3;
	else if(e==4)
		e_move <= e4;
	else if(e==5)
		e_move <= e5;
	else if(e==6)
		e_move <= e6;
	else if(e==7)
		e_move <= e7;
	else
		e_move <= init;

	if(f == 0)
		f_move <= f0;
	else if(f==1)
		f_move <= f1;
	else if(f==2)
		f_move <= f2;
	else if(f==3)
		f_move <= f3;
	else if(f==4)
		f_move <= f4;
	else if(f==5)
		f_move <= f5;
	else if(f==6)
		f_move <= f6;
	else if(f==7)
		f_move <= f7;
	else
		f_move <= init;

	if(g == 0)
		g_move <= g0;
	else if(g==1)
		g_move <= g1;
	else if(g==2)
		g_move <= g2;
	else if(g==3)
		g_move <= g3;
	else if(g==4)
		g_move <= g4;
	else if(g==5)
		g_move <= g5;
	else if(g==6)
		g_move <= g6;
	else if(g==7)
		g_move <= g7;
	else
		g_move <= init;

	if(h == 0)
		h_move <= h0;
	else if(h==1)
		h_move <= h1;
	else if(h==2)
		h_move <= h2;
	else if(h==3)
		h_move <= h3;
	else if(h==4)
		h_move <= h4;
	else if(h==5)
		h_move <= h5;
	else if(h==6)
		h_move <= h6;
	else if(h==7)
		h_move <= h7;
	else
		h_move <= init;
end
always@(posedge CLK_x)       //charator move detect
begin
   if(left)                          //move left
	   begin
	   if(position==0)
		   position <= position;
		else
		   position <= position - 1;
		end
	else if(right)                    //move right
	   begin
		if(position==7)
		   position <= position;
		else
		   position <= position + 1;
		end
	else
	   position <= position;
end
always@(posedge CLK_div)	  //不斷畫面更新
begin
   //counter
   if(seg_cnt == 4'b0111)
		seg_cnt <= 4'b1110;
	else
		seg_cnt <= {seg_cnt[2:0],1'b1};
	//	
   if(cnt>=7)
	   cnt <= 0;
	else
      cnt <= cnt + 1;
	COMM <= {1'b1, cnt};
	
	//reset game
	if(reset)
	   begin
		   gameover <= 0;
			DATA_R <= 8'b11111111;
			DATA_B <= 8'b11111111;
			DATA_G <= 8'b11111111;
			life <= 1;
		end
	if(gameover == 0)
	begin
			//counter
		if(C2_count == 1 && C3_count == 8 && C4_count == 0)
		   gameover <= 1;
		case({seg_cnt})
		4'b1110:seg_count <= 3;
		4'b1101:seg_count <= 0;
		4'b1011:seg_count <= 1;
		4'b0111:seg_count <= 2;
		endcase
	   index[0] <= C4_count;
		index[1] <= C3_count;
		index[2] <= C2_count;
		index[3] <= C1_count;
		A_count <= index[seg_count];
		case(A_count)
		4'b0000: {seg} = 7'b0000001;
		4'b0001: {seg} = 7'b1001111;
		4'b0010: {seg} = 7'b0010010;
		4'b0011: {seg} = 7'b0000110;
		4'b0100: {seg} = 7'b1001100;
		4'b0101: {seg} = 7'b0100100;
		4'b0110: {seg} = 7'b0100000;
		4'b0111: {seg} = 7'b0001101;
		4'b1000: {seg} = 7'b0000000;
		4'b1001: {seg} = 7'b0000100;

		default: {seg} = 7'b1111111;
		endcase
		//end counter
		if(position==0)
			DATA_B <= X0[cnt];
		else if(position==1)
			DATA_B <= X1[cnt];
		else if(position==2)
			DATA_B <= X2[cnt];
		else if(position==3)
			DATA_B <= X3[cnt];
		else if(position==4)
			DATA_B <= X4[cnt];
		else if(position==5)
			DATA_B <= X5[cnt];
		else if(position==6)
			DATA_B <= X6[cnt];
		else if(position==7)
			DATA_B <= X7[cnt];
		else
			DATA_B <= DATA_B;
		DATA_R <= a_move[cnt] & b_move[cnt] & c_move[cnt] & d_move[cnt] & e_move[cnt] & f_move[cnt] & g_move[cnt] & h_move[cnt];
		DATA_G <= a_s_move[cnt] & b_s_move[cnt] & c_s_move[cnt] & d_s_move[cnt] & e_s_move[cnt] & f_s_move[cnt] & g_s_move[cnt] & h_s_move[cnt];
		//collision
      if(a_shoot != 0 && a != 8)
		   begin
			   if((a_shoot <= (a + 1)) || ((a_shoot - 1) <= (a + 1)))
				   a_reset <= 1;
			end
		else
	      a_reset <= 0;
		if(b_shoot != 0 && b != 8)
		   begin
			   if((b_shoot <= (b + 1)) || ((b_shoot - 1) <= (b + 1)))
				   b_reset <= 1;
			end
		else
	      b_reset <= 0;
	  	if(c_shoot != 0 && c != 8)
		   begin
			   if((c_shoot <= (c + 1)) || ((c_shoot - 1) <= (c + 1)))
				   c_reset <= 1;
			end
		else
	      c_reset <= 0;
		if(d_shoot != 0 && d != 8)
		   begin
			   if((d_shoot <= (d + 1)) || ((d_shoot - 1)<= (d + 1)))
				   d_reset <= 1;
			end
		else
	      d_reset <= 0;
		if(e_shoot != 0 && e != 8)
		   begin
			   if((e_shoot <= (e + 1)) || ((e_shoot - 1) <= (e + 1)))
				   e_reset <= 1;
			end
		else
	      e_reset <= 0;
		if(f_shoot != 0 && f != 8)
		   begin
			   if((f_shoot <= (f + 1)) || ((f_shoot - 1) <= (f + 1)))
				   f_reset <= 1;
			end
		else
	      f_reset <= 0;
		if(g_shoot!= 0 && g != 8)
		   begin
			   if((g_shoot <= (g + 1) ) || ((g_shoot - 1) <= (g + 1)))
				   g_reset <= 1;
			end
		else
	      g_reset <= 0;
		if(h_shoot!= 0 && h != 8)
		   begin
			   if((h_shoot <= (h + 1)) || ((h_shoot - 1) <= (h + 1)))
				   h_reset <= 1;
			end
		else
	      h_reset <= 0;
	   // collision end
		if((DATA_B[7]==0) && DATA_R[7]==0)
			life <= life - 1;      //扣血
		if(life == 0)
		   gameover <= 1;         //遊戲結束
		   
	end
	else
	   begin
	      DATA_B <= ez[cnt];
		   DATA_R <= ez[cnt];
	   end
end
	

endmodule

module divfreq(input CLK, output reg CLK_div); //畫面更新用 1000Hz
reg [24:0] Count;
always @(posedge CLK)
begin
if(Count > 50000)
begin
Count <= 25'b0;
CLK_div <= ~CLK_div;
end
else
Count <= Count + 1'b1;
end
endmodule

module divfreq_x(input CLK, output reg CLK_x); //角色移動檢測用 間隔約0.08秒
reg [24:0] Count;
always @(posedge CLK)
begin
if(Count > 4000000)
begin
Count <= 25'b0;
CLK_x <= ~CLK_x;
end
else
Count <= Count + 1'b1;
end
endmodule

module divfreq_1(input CLK, output reg CLK_1); //掉落物 0.5Hz
reg [24:0] Count;
always @(posedge CLK)
begin
if(Count > 12500000)
begin
Count <= 25'b0;
CLK_1 <= ~CLK_1;
end
else
Count <= Count + 1'b1;
end
endmodule

module divfreq_rand(input CLK, output CLK_rand); //掉落物位置選擇
reg [24:0] Count;
integer s;
always @(posedge CLK)
begin
if(Count > 2500000)
begin
   Count <= 25'b0;
	CLK_rand <= ~CLK_rand;
end
else
   Count <= Count + 1'b1;
end
endmodule

module divfreq_shoot(input CLK, output CLK_shoot); //shoot

reg [24:0] Count;
integer s;
always @(posedge CLK)
begin
if(Count > 12500000)
begin
   Count <= 25'b0;
	CLK_shoot <= ~CLK_shoot;
end
else
   Count <= Count + 1'b1;
end
endmodule

module divfreq_s(input CLK, output CLK_s); 
reg [24:0] Count;
integer s;
always @(posedge CLK)
begin
if(Count > 4000000)
begin
   Count <= 25'b0;
	CLK_s <= ~CLK_s;
end
else
   Count <= Count + 1'b1;
end
endmodule

module divfreq_2(input CLK, output reg CLK_2);  //clock of counter
reg [24:0] Count;
always @(posedge CLK)
begin
if(Count > 25000000)
begin
Count <= 25'b0;
CLK_2 <= ~CLK_2;
end
else
Count <= Count + 1'b1;
end
endmodule
