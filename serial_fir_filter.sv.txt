// Signed adder

module signed_adder
#(parameter WIDTH=16)
(
	input signed [WIDTH-1:0] dataa,
	input signed [WIDTH-1:0] datab,
	input cin,
	output [WIDTH:0] result
);

	assign result = dataa + datab + cin;

endmodule

// Quartus Prime Verilog Template
// Signed multiply

module signed_multiply
#(parameter WIDTH=16)
(
	input signed [WIDTH-1:0] dataa,
	input signed [WIDTH-1:0] datab,
	output [2*WIDTH-1:0] dataout
);

	 assign dataout = dataa * datab;

endmodule


module register
#(parameter WIDTH=16)
(
	input signed [WIDTH-1:0] dataa,
	input clk,
	input reset_n,
	output logic signed [WIDTH-1:0] dataout
);

always_ff@ (posedge clk)
	if(reset_n == 1'b0)
		dataout <= 0;
			
	else				
		dataout <= dataa;

endmodule


module serial_fir_filter
#(parameter WIDTH=16)
(
	input logic clk, input logic reset_n,
	input logic signed[WIDTH-1:0] d,
	output logic signed[WIDTH-1:0] q
);
			  
logic signed[WIDTH-1:0] delay[WIDTH-1:0];
logic signed[2*WIDTH-1:0] prod;
logic signed[WIDTH:0] sum;
			  
logic signed [WIDTH-1:0] lut_rom[0:WIDTH-1];			  
logic signed[WIDTH-1:0] coefficient;
			  
assign delay[0] = d;
integer i = 0;
			  
initial
	begin: lut_initalization
		$readmemb("C:/intelFPGA_lite/18.1/serial_fir_filter/rom-data.txt", lut_rom);
	end
					
genvar n;
generate
	for(n = 1; n < WIDTH; n = n + 1)
		begin:generator
			register reg_inst0(.dataa(delay[n-1]),
									 .clk(clk),
									 .reset_n(reset_n),
									 .dataout(delay[n]));
		end
endgenerate
			 
always_ff@(posedge clk)
	for(i = 0; i <= WIDTH - 1; i = i+ 1)
		begin: generate_address
			coefficient  <= lut_rom[i];
		end
					
signed_multiply inst(.dataa(delay[WIDTH-1]),
				         .datab(coefficient),
							.dataout(prod));
				
signed_adder adder_fir(.dataa($signed(prod[2 * WIDTH-2:WIDTH-1])),
							  .datab(q),
							  .cin(0),
							  .result(sum));

register reg_inst1(.dataa($signed(sum[WIDTH:1])),
						 .clk(clk),
						 .reset_n(reset_n),
						 .dataout(q));

endmodule
