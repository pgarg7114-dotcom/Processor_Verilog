module testbench;
reg clk1, clk2;
integer k;
processor DUT(clk1, clk2);
initial
begin
clk1=0; 
forever    // generating two phase clock pulses
begin
#5 clk1=1;
#5 clk1=0;
#10 clk1=0;
end
end
initial
begin
clk2=0;
forever
begin
#15 clk2=1;
#5 clk2=0;
end
end
initial
begin
for(k=0; k<32; k=k+1)
DUT.regfile[k]=k;
end
initial
begin
DUT.mem[0]=32'h2801000a;
DUT.mem[1]=32'h28020014;
DUT.mem[2]=32'h28030019;
DUT.mem[3]=32'h0ce77800;
DUT.mem[4]=32'h00222000;
DUT.mem[5]=32'h0ce77800;
DUT.mem[6]=32'h0ce77800;
DUT.mem[7]=32'h00832800;
DUT.mem[8]=32'hfc000000;
end
initial
begin
DUT.HALTED=0;
DUT.PC=0;
DUT.TAKEN_BRANCH=0;
#280 for(k=0; k<6; k=k+1)
begin
$display("R%1d - %2d", k, DUT.regfile[k]);
end
#10 $finish;
end
endmodule
