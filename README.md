# -EG4S20
Based on EG4S20, the relevant code for FPGA competition
module ad9364_top(
    input clk_100m,
    output txdata,
    input  rxdata
)
 
 
clk_wiz clk_wiz_xst
 
   (
    // Clock out ports
    .clk_40m(clk_40m),     // output clk_40m
 
    .clk_1p8432(clk_1p8432),
 
   // Clock in ports
 
    .clk_in(clk_100m));      // input clk_in
 
 
 
rst_generator rst_xst(
	.clk_in(clk_40m),
	.rst_n(rst_n)
);
 
 
fftdata fft_data_xst(
	.clk(clk_1p8432),
    .clkfftd(clk_40m),
    .flag1(tx_rdy),
    .fftdata(tx_data),
    .reset(uart_rst),
    .write(uart_wt),
    .tx_data(uart_data),
    .writebegin(writebegin)
);
 
 
 
uart uart_xst(
	.mclkx16(clk_1p8432), 
	.reset(uart_rst), 
	.read(), 
	.write(uart_wt), 
	.data(), 
	.txdata(uart_data),
	.sin(), 
	.sout(uart_sout), 
	.rxrdy(), 
	.txrdy(), 
	.parity_error(), 
	.framing_error(), 
	.overrun()
		
);
 
这是AT指令存储以及控制uart的模块代码

`timescale 1ns / 1ps
//////////////////////////////////////////////////////////////////////////////////
// 
// editor: loving_Qi
// 
// Create Date: 2018/05/31 10:34:12
// Design Name: 
// Module Name: control_udp
// Project Name: 
// Target Devices: 
// Tool Versions: 
// Description: 
// 
// Dependencies: 
// 
// Revision:
// Revision 0.01 - File Created
// Additional Comments:
// 
//////////////////////////////////////////////////////////////////////////////////
module fftdata(input clk,
              input clkfftd,
              input flag1,
              input [7:0]fftdata,
			  input rst_n,
              output reset,
              output write,
              output[7:0] tx_data,
			  output writebegin
              
    );
reg reset=0;
reg write;
reg writebegin=0;
reg flag=0;
reg readover=0;
reg [31:0] j=0;
reg [31:0] count=0;
reg [31:0] count1=0;
reg [7:0]  tx_data;
reg [31:0] count_w=0;
reg [31:0] count_d=0;
reg [31:0] count_s=0;
reg [7:0] memo[4178:0];
//reg [7:0] memo1[2499999:0];
always@(posedge clkfftd )
begin
//if(!rst_n)
//begin
//	j <= 0;
//	flag <= 0;
//end
//else
//begin
 memo[0]<=8'h41;    
memo[1]<=8'h54;    
memo[2]<=8'h2b;
memo[3]<=8'h52;
memo[4]<=8'h53;
memo[5]<=8'h54;
memo[6]<=8'h0d;
memo[7]<=8'h0a;
memo[8]<=8'h41;
memo[9]<=8'h54;
memo[10]<=8'h2b;
memo[11]<=8'h43;
memo[12]<=8'h57;
memo[13]<=8'h4d;
memo[14]<=8'h4f;
memo[15]<=8'h44;
memo[16]<=8'h45;
memo[17]<=8'h3d;
memo[18]<=8'h33;
memo[19]<=8'h0d;
memo[20]<=8'h0a;
memo[21]<=8'h41;    
memo[22]<=8'h54;    
memo[23]<=8'h2b;
memo[24]<=8'h43;
memo[25]<=8'h49;
memo[26]<=8'h50;
memo[27]<=8'h53;
memo[28]<=8'h54;
memo[29]<=8'h41;
memo[30]<=8'h52;
memo[31]<=8'h54;
memo[32]<=8'h3d;
memo[33]<=8'h22;
memo[34]<=8'h54;
memo[35]<=8'h43;
memo[36]<=8'h50;
memo[37]<=8'h22;
memo[38]<=8'h2c;
memo[39]<=8'h22;
memo[40]<=8'h31;
memo[41]<=8'h39;
memo[42]<=8'h32;
memo[43]<=8'h2e;
memo[44]<=8'h31;
memo[45]<=8'h36;
memo[46]<=8'h38;
memo[47]<=8'h2e;
memo[48]<=8'h34;
memo[49]<=8'h2e;
memo[50]<=8'h32;
memo[51]<=8'h22;
memo[52]<=8'h2c;
memo[53]<=8'h38;
memo[54]<=8'h30;
memo[55]<=8'h38;
memo[56]<=8'h34;
memo[57]<=8'h0d;
memo[58]<=8'h0a;
memo[59]<=8'h41;
memo[60]<=8'h54;
memo[61]<=8'h2b;
memo[62]<=8'h43;   
memo[63]<=8'h49;
memo[64]<=8'h50;
memo[65]<=8'h4d;
memo[66]<=8'h4f;
memo[67]<=8'h44;
memo[68]<=8'h45;
memo[69]<=8'h3d;
memo[70]<=8'h31;
memo[71]<=8'h0d;
memo[72]<=8'h0a;
memo[73]<=8'h41;
memo[74]<=8'h54;
memo[75]<=8'h2b;
memo[76]<=8'h43;
memo[77]<=8'h49;
memo[78]<=8'h50;
memo[79]<=8'h53;
memo[80]<=8'h45;
memo[81]<=8'h4e;
memo[82]<=8'h44;
memo[83]<=8'h0d;
memo[84]<=8'h0a;
if(flag1==1)
    begin
        
        if(j<4094)
            begin 
                memo[j+85]<=fftdata;
                 j<=j+1;
                flag<=0;
            end
        else
            begin
                j<=0;
                flag<=1;
            end
    end
else
	begin
    j<=0;
	flag<=1;	
	end
end
//end
always@(posedge clk )
begin
//if(!rst_n)
//	begin
//	count1 <= 0;
//	reset <=0;
//	end
//else
//	begin
		if(count1>5&&count1<11)
		begin
			reset<=1;
			count1<=count1+1;
		end
		else if(count1<6)
		count1<=count1+1; 
		else
		reset<=0;
	end
//end
always@(posedge clk )
begin
//if(!rst_n)
//begin
//	readover <= 0;
//	count_w <= 0;
//	count_d <= 0;
//	count_s <= 0;
//	write <= 1;
//	writebegin <= 0;
//end
//else
//begin
    if(flag==0)
        begin
        readover<=0;
        count_w<=0;
        end
	else
	begin
      if(count_w<=7&&readover==0)
        begin
            write<=1'b1; 
            if(count_w==7)
                begin
//                    tx_data<=(count_d<92)?memo[count_d]:memo1[count_d-1125];   
               tx_data<=memo[count_d];     
                    if(count_d<86&&memo[count_d-1]==8'h0a && count_s<8000000)
                        begin
                        count_s<=count_s+1;
                        write<=1'b1;
                        end
					else if(count_d==84)
						begin
						writebegin<=1;
						count_d<=count_d+1;
						count_w<=count_w+1;
						end
                    else
                        begin
                        count_d<=count_d+1;
                        count_w<=count_w+1;
                        count_s<=0;
                           if (count_d>4178)
                            begin
                            count_d<=85;
                            count_w<=200;
                            readover<=1;
                            end
                       end
                 end
              else
              count_w<=count_w+1;
        end
      else if(count_w>7 &&count_w<=19&&readover==0)
        begin
            write<=1'b0;
            count_w<=count_w+1;
        end
    else if(count_w>19 &&count_w<=178&&readover==0)
        begin
            write<=1'b1;
            count_w<=count_w+1;
        end
    else if (count_w==179&&readover==0)
            count_w<=0; 
   // tx_data=0;
   end
end
//end
 
endmodule
下面是UART部分Verilog代码

UART的top文件

/******************************************************************
*
* 	File:  		uart.v
*
* 	Purpose: 	Top level UART description.  UART implements
*			a full duplex function.  This interface
*			interprets processor read/write parallel bus
*			protocol and translates to serial interface.
*		
**********************************************************************/
 
 
 
`timescale 1ns / 100ps
 
 
 
module uart (   mclkx16, reset, read, write, data, txdata,
 
		sin, sout, rxrdy, txrdy, parity_error, framing_error, overrun);
 
               
 
input      	mclkx16;		// Input clock, 16 x baud rate clock
 
input       	read;		   	// read strobe input
 
input       	write;		   	// write strobe input
 
input	      	reset;		   	// Master reset input
 
input       [7:0] txdata;   wire  [7:0] txdata; //trans data
 
output 		[7:0] data;		// Bidirectional data bus
 
 
 
 
 
// Receiver input signal, error and status flags
 
input	    sin;	                     		// Receive  data line input from IrDA interface
 
output      rxrdy;      	wire     rxrdy;      	// Data ready to be read
 
output      parity_error;  	wire     parity_error;  // Parity error flag
 
output      framing_error; 	wire     framing_error; // Framing error flag
 
output      overrun;      	wire     overrun;    	// Overrun error flag
 
wire        [7:0] rxdata;		     		// Intermediate output signals from receiver
 
 
 
 
 
// Transmitter output signal and status flag
 
output      sout;       wire     sout;       // Transmit data line output
 
output      txrdy;      wire     txrdy;      // Transmitter ready for next byte
 
 
 
 
 
//Instantiation of the transmitter module
 
txmit tx (mclkx16, write, reset, sout, txrdy, txdata);
 
 
 
// Instantiation of the receiver module
 
rxcver  rx (mclkx16, read, sin, reset, rxrdy, parity_error, framing_error, overrun, rxdata);
 
 
 
// Drives the data bus during data read, otherwise tri-state the data bus
 
assign 		data = !read ? rxdata : 8'bzzzzzzzz;
 
 
 
endmodule
UART发送模块verliog代码

/******************************************************************
*
* 	File:  		txmit.v
*
* 	Purpose: 	UART transmit description.  Interprets 
*			processor read/write parallel bus cycles 
*			and converts to output serial data.
*		
**********************************************************************/
 
 
 
`timescale 1ns / 100ps
 
 
 
module txmit 
 
(mclkx16, write, reset, sout, txrdy, data);
 
 
 
input    mclkx16;		// Input clock, 16 x baudrate clock 
 
input    write;		      	// Transmit write signal
 
input	 reset;		      	// Reset
 
output   sout;   reg   sout;   	// Transmit data output	
 
output   txrdy;		      	// Transmitter ready to recieve next byte to be send
 
input    [7:0] data;       	// 8-bit input data bus
 
 
 
reg      write1, write2;      	// Delayed write signals
 
reg      txdone1;        	// txdone delayed signal
 
 
 
 
 
// Transmit shift register bits
 
reg      [7:0] thr;			// Transmit hold register
 
reg      [7:0] tsr;           		// Transmit shift register, used for shifting out data to sout
 
reg      tag1, tag2;		      	// Tag bits used for detecting, when the tsr is empty
 
wire	 paritymode = 1'b1; 		// Initialized to 1 = odd parity, 0 = even parity
 
reg      txparity;		   	// Parity generation register
 
 
 
 
 
// Transmit clock and other control signals
 
reg      txclk;            		// Transmit clock, i.e. baudrate clock = mclkx16 / 16
 
wire     txdone;		      	// Set to high, when shifting of byte is done
 
wire     paritycycle;	   		// Set to high, one cycle next to last shift cycle
 
reg      txdatardy;        		// Set to high, when data is ready in transmit hold register
 
reg      [2:0] cnt;           		// Counter used for generating the internal baud rate clock
 
 
 
 
 
// Paritycycle = 1 on next to last cycle, this means when tsr[1] gets tag2
 
assign   paritycycle = tsr[1] && !(tag2 || tag1 || tsr[7] || tsr[6] || tsr[5] || tsr[4] || tsr[3] || tsr[2]);
 
 
 
// txdone = 1 when done shifting, this means when sout gets tag2
 
assign   txdone = !(tag2 || tag1 || tsr[7] || tsr[6] || tsr[5] || tsr[4] || tsr[3] || tsr[2] || tsr[1] || tsr[0]);
 
 
 
// Ready for new data to be written, when no data is in transmit hold register
 
assign   txrdy = !txdatardy;
 
 
 
 
 
// Latch data[7:0] into the transmit hold register at falling edge of write
 
always @(write or data)
 
   if (~write) 
 
   	thr = data;
 
 
 
 
 
// Toggle txclk every 8 counts, which divides the clock by 16, to generate the baud clock
 
always @(posedge mclkx16 or posedge reset)
 
if (reset)
 
	begin
 
	txclk <= 1'b0;
 
	cnt <= 3'b000;
 
	end
 
else
 
	begin
 
	if (cnt == 3'b000)  
 
	    txclk <= !txclk; 
 
	cnt <= cnt + 1;
 
   end
 
 
 
 
 
// Shifting out data to sout
 
always @(posedge txclk or posedge reset)
 
if (reset)
 
	begin
 
	tsr      <= 8'h00;	   	// Reset transmit shift register
 
	tag2     <= 1'b0;	      	// Reset tag bit
 
	tag1     <= 1'b0;	      	// Reset tag bit
 
	txparity <= 1'b0;		// Reset txparty bit
 
	sout     <= 1'b1;	      	// Idle -> set start bit high
 
	end
 
else
 
	begin
 
		if (txdone && txdatardy)
 
			begin
 
			tsr        <= thr;	      		// Load thr to shift register
 
			tag2       <= 1'b1;	      		// Set tag bits for detecting when shifting is done 
 
			tag1       <= 1'b1;	      		// Set tag bits for detecting when shifting is done
 
			txparity   <= paritymode;		// Set parity mode -> 0 = even parity, 1 = odd parity
 
	    		sout       <= 1'b0;	      		// Set start bit low
 
			end
 
		
 
		else
 
	   		begin   
 
			tsr     <= tsr >> 1;      			// Send LSB first
 
			tsr[7]  <= tag1;          		 	// Set tsr[7] = tag1  
 
			tag1    <= tag2;          		 	// Set tag1 = tag2
 
			tag2    <= 1'b0;          		 	// Set tag2 = 0
 
			txparity <= txparity ^ tsr[0]; 			// Generate parity
 
         
 
         
 
	    		// Shift out data or parity bit or stop/idle bit.
 
	     		if (txdone)
 
		   		sout <= 1'b1;	     		// Output stop/idle bit
 
	    		else if (paritycycle)
 
		   		sout <= txparity;   		// Output parity bit
 
	     		else
 
				sout <= tsr[0];     		// Shift out data bit
 
	    	end
 
	end
 
 
 
 
 
always @(posedge mclkx16 or posedge reset)
 
if (reset) 
 
	begin
 
	txdatardy <= 1'b0;
 
	write2 <= 1'b1;
 
	write1 <= 1'b1;		
 
	txdone1 <= 1'b1;		            
 
	end
 
else
 
	begin
 
	if (write1 &&  !write2)
 
	   	txdatardy  <= 1'b1;           		// Set txdatardy on rising edge of write
 
 
 
	else if (!txdone &&  txdone1)
 
	     	txdatardy  <= 1'b0;			// Falling edge of txdone indicated the thr is loaded in the tsr
 
 
 
	// Generate delayed versions of write and txdone signals for edge detection.
 
	write2 <= write1;
 
	write1 <= write;
 
	txdone1 <= txdone;
 
	
 
	end 
 
 
 
endmodule
UART接收模块verilog代码

/******************************************************************
*
* 	File:  		rxcver.v
*
* 	Purpose: 	Main UART receiver logic module. Receives 
*			incoming serial data and present parallel
*			byte of data to system.  Includes rxrdy control
*			signals for handshaking of system bus.  Includes
*			control flags for parity, overrun data, and
*			framing errors.
*		
**********************************************************************/
 
 
 
`timescale 1ns / 100ps
 
 
 
module rxcver (mclkx16, read, sin, reset, rxrdy, parity_error, framing_error, overrun, rxdata);
 
 
 
input       	mclkx16;		// Input clock, 16 x baudrate clock
 
input       	read;			// Read control signal
 
input	      	sin;		  	// Receive input serial signal  
 
input	      	reset;			// Reset
 
 
 
 
 
// receive status & error signals
 
output      rxrdy;		   				   // Data received
 
output      parity_error; 		reg	parity_error;	   // Parity error control signal
 
output      framing_error;		reg	framing_error;	   // Framing error detect signal
 
output      overrun;			reg	overrun;	   // Overrun error detect signal
 
 
 
// 8 bit latched output data bus.
 
output      [7:0] rxdata;		reg 	[7:0]rxdata;	   	// 8-bit output data bus
 
 
 
// Internal control signals.
 
reg         [3:0] rxcnt;    					  	// Count of clock cycles
 
reg         rx1, read1, read2, idle1, hunt;     			// Delayed version signals
 
 
 
// Receive shift register bits
 
reg		[7:0] rhr;	    	     	 // Receive hold register
 
reg 		[7:0] rsr;  	      	 	 // Receive shift register
 
reg    		rxparity;	         	 // Received parity bit
 
reg   		paritygen;		         // Parity generated from received data
 
reg      	rxstop;	   	      		 // Received data stop bit
 
 
 
// Receive clock and control signals.
 
reg       	rxclk;     			// Receive data shift clock
 
reg      	idle;     	   		// idle = 1 when receiver is idle
 
reg   		rxdatardy;     			// rsdatardy = 1 when data is ready to be read
 
 
 
 
 
// Idle signal enables rxclk generation - idle = 0 when not shifting data
 
// idle = 1 when low "rxstop" bit = rsr[0]
 
always @(posedge rxclk or posedge reset)
 
   begin
 
   	if (reset)
 
      		idle <= 1'b1;
 
      	else 
 
      		idle <= !idle && !rsr[0];
 
	end
 
 
 
 
 
// Synchronizing rxclk to the centerpoint of low leading startbit
 
always @(posedge mclkx16)
 
begin
 
 
 
	// A start bit is eight clock times with sin=0 after a falling edge of sin
 
	if (reset)
 
	    hunt <= 1'b0;
 
	else if (idle && !sin && rx1 )	
 
	    	hunt <= 1'b1;					// Look for falling edge of sin
 
	else if (!idle || sin )			
 
	    	hunt <= 1'b0;					// Stop when shifting in data, or a 1 is found on sin
 
   
 
   	if (!idle || hunt)					
 
	   	rxcnt <= rxcnt + 1;				// Count clocks when not idle, or looking for start bit
 
	else									
 
	   	rxcnt <= 4'b0001;				// Hold rxcnt = 1, when idle and waiting for falling edge of sin
 
 
 
   	rx1 <= sin;						// Looking for falling edge detect on sin
 
   
 
   	rxclk <= rxcnt[3];               			// rxclk = mclkx16 / 16
 
 
 
end
 
 
 
// When not idle, sample data at the sin input and create parity
 
always @(posedge rxclk or posedge reset)
 
if (reset)
 
	begin
 
	rsr        <= 8'b11111111;		// Initialize shift register
 
	rxparity   <= 1'b1;        		// Set to 1 -> for data shifting          
 
	paritygen  <= 1'b1;           		// Set to 1 -> odd parity mode
 
	rxstop     <= 1'b0;         		// Controls idle = 1, when rsr[0] gets rxstop bit
 
	end
 
	
 
else
 
  	begin												      
 
	if (idle)
 
		begin
 
		rsr        <= 8'b11111111;		// Initialize shift register
 
		rxparity   <= 1'b1;        		// Set to 1 -> for data shifting         
 
		paritygen  <= 1'b1;           		// Set to 1 -> odd parity mode
 
		rxstop     <= 1'b0;         		// Controls idle = 1, when rsr[0] gets rxstop bit
 
		end
 
		
 
	else
 
		begin
 
		rsr         <= rsr >> 1;            	// Right shift sin shift register   
 
		rsr[7]      <= rxparity;            	// Load rsr[7] with rxparity
 
		rxparity    <= rxstop;              	// Load rxparity with rxstop
 
		rxstop      <= sin;                  	// Load rxstop with sin
 
        	paritygen   <= paritygen ^ rxstop;  	// Generate running parity
 
        	end
 
	end
 
 
 
 
 
// Generate status & error flags
 
always @(posedge mclkx16 or posedge reset)
 
if (reset) 
 
	begin
 
	rhr         	<= 8'h00;
 
	rxdatardy   	<= 1'b0;
 
	overrun	    	<= 1'b0;
 
	parity_error   	<= 1'b0;
 
	framing_error  	<= 1'b0;
 
	idle1       	<= 1'b1; 
 
	read2       	<= 1'b1; 
 
	read1       	<= 1'b1; 
 
	end
 
else	
 
	begin
 
	
 
	// Look for rising edge of idle and update output registers
 
	if (idle && !idle1)				   
 
	 	begin
 
		if (rxdatardy)
 
			overrun <= 1'b1;				// Overrun error, if previous data still in holding register
 
		else
 
			begin
 
			overrun <= 1'b0;				// No overrun error, since holding register is empty
 
			rhr <= rsr;					// Update holding register with contens of shift register
 
			parity_error <= paritygen;    			// paritygen = 1, if parity error
 
			framing_error <=  !rxstop;			// framing_error, if stop bit is not 1
 
			rxdatardy <= 1'b1;				// Data is ready for reading flag
 
			end
 
		end
 
 	   
 
 	   
 
 	// Clear error and data registers when data is read
 
	if (!read2 &&  read1)
 
		begin 					
 
	   	rxdatardy  	<= 1'b0;
 
	  	parity_error  	<= 1'b0;
 
	   	framing_error 	<= 1'b0;
 
	   	overrun    	<= 1'b0;
 
	   	end 
 
 
 
		idle1 <= idle;				        // Edge detect on idle signal
 
		read2 <= read1;	   				// 2 cycle delayed version of read - edge detection
 
		read1 <= read;					// 1 cycle delayed version of read - edge detection
 
   	end
 
 
 
assign    rxrdy = rxdatardy;		   		// Receive data ready output signal
 
 
 
always @(read or rhr)				   	// Latch data output when read goes low
 
if (~read) 
 
	rxdata = rhr; 
 
 
 
endmodule
