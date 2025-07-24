%%writefile stepone.v
module stepone ( output one );

    // Output is always logic high (1)
    assign one = 1'b1;

endmodule

%%writefile tb.v
`timescale 1ns/1ns

module tb;
    wire out_tb;

    // Instantiate the stepone module
    stepone uut (
        .one(out_tb)
    );

    initial begin
        $dumpfile("tb.vcd");  // Make sure we generate tb.vcd
        $dumpvars(0, tb);     // Dump all signals in tb module

        // Monitor time and output signal
        $monitor("Time = %0t ns | Output = %b", $time, out_tb);

        #10 $finish;  // Run simulation for 10 time units
    end
endmodule

!iverilog -o tb.out stepone.v tb.v  # Compile the updated testbench
!vvp tb.out                         # Run simulation, should generate tb.vcd


%%waveform tb.vcd
sign_list = ['tb.out_tb']  # Ensure this matches the signal name in the testbench
time_begin = 0
time_end = 10
base = 'bin'