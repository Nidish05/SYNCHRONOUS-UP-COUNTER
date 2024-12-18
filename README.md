### SYNCHRONOUS-UP-COUNTER

**AIM:**

To implement 4 bit synchronous up counter and validate functionality.

**SOFTWARE REQUIRED:**

Quartus prime

**THEORY**

**4 bit synchronous UP Counter**

If we enable each J-K flip-flop to toggle based on whether or not all preceding flip-flop outputs (Q) are “high,” we can obtain the same counting sequence as the asynchronous circuit without the ripple effect, since each flip-flop in this circuit will be clocked at exactly the same time:

![image](https://github.com/naavaneetha/SYNCHRONOUS-UP-COUNTER/assets/154305477/d5db3fa0-e413-404c-b80e-b2f39d82e7e8)


![image](https://github.com/naavaneetha/SYNCHRONOUS-UP-COUNTER/assets/154305477/52cb61eb-d04b-442d-810c-31185a68410b)

Each flip-flop in this circuit will be clocked at exactly the same time.
The result is a four-bit synchronous “up” counter. Each of the higher-order flip-flops are made ready to toggle (both J and K inputs “high”) if the Q outputs of all previous flip-flops are “high.”
Otherwise, the J and K inputs for that flip-flop will both be “low,” placing it into the “latch” mode where it will maintain its present output state at the next clock pulse.
Since the first (LSB) flip-flop needs to toggle at every clock pulse, its J and K inputs are connected to Vcc or Vdd, where they will be “high” all the time.
The next flip-flop need only “recognize” that the first flip-flop’s Q output is high to be made ready to toggle, so no AND gate is needed.
However, the remaining flip-flops should be made ready to toggle only when all lower-order output bits are “high,” thus the need for AND gates.

**Procedure**
```
1.Initialize the shift register to a known state (e.g., all zeros).

2.Input a bit serially into the shift register.

3.Shift the contents of the register one position to the right (or left).

4.Output the shifted bit from the last stage of the register.

5.Repeat steps 2-4 for each bit you want to input and shift.
```

/* write all the steps invloved */

**PROGRAM**
```
module (
    input clk,    // Clock signal
    input rst,    // Reset signal (active high)
    output [3:0] q // 4-bit output
);

    wire [3:0] j, k; // J and K inputs for each JK flip-flop
    wire [3:0] t;    // Toggle signal for each flip-flop

    // Generate the toggle signals for each stage
    assign j[0] = 1'b1; // First flip-flop toggles on every clock pulse
    assign k[0] = 1'b1;
    assign t[0] = q[0]; // Output of the first flip-flop

    assign j[1] = q[0]; // Second flip-flop toggles on q[0] high
    assign k[1] = q[0];
    assign t[1] = q[1];

    assign j[2] = q[0] & q[1]; // Third flip-flop toggles on q[1:0] high
    assign k[2] = q[0] & q[1];
    assign t[2] = q[2];

    assign j[3] = q[0] & q[1] & q[2]; // Fourth flip-flop toggles on q[2:0] high
    assign k[3] = q[0] & q[1] & q[2];
    assign t[3] = q[3];

    // Instantiate 4 JK flip-flops
    jk_flipflop jk0 (.clk(clk), .rst(rst), .j(j[0]), .k(k[0]), .q(q[0]));
    jk_flipflop jk1 (.clk(clk), .rst(rst), .j(j[1]), .k(k[1]), .q(q[1]));
    jk_flipflop jk2 (.clk(clk), .rst(rst), .j(j[2]), .k(k[2]), .q(q[2]));
    jk_flipflop jk3 (.clk(clk), .rst(rst), .j(j[3]), .k(k[3]), .q(q[3]));

endmodule

// JK flip-flop module
module jk_flipflop (
    input clk,    // Clock signal
    input rst,    // Reset signal (active high)
    input j,      // J input
    input k,      // K input
    output reg q  // Q output
);
    always @(posedge clk or posedge rst) begin
        if (rst) begin
            q <= 1'b0; // Reset output to 0
        end else begin
            case ({j, k})
                2'b00: q <= q;       // No change
                2'b01: q <= 1'b0;    // Reset
                2'b10: q <= 1'b1;    // Set
                2'b11: q <= ~q;      // Toggle
            endcase
        end
    end
endmodule
```

/* Program for flipflops and verify its truth table in quartus using Verilog programming. 

Developed by:Nidishkumar S



RegisterNumber:24002777
*/

**RTL LOGIC UP COUNTER**
![Screenshot 2024-12-18 141411](https://github.com/user-attachments/assets/be89b48e-5e8e-4da9-9841-e648b1d08429)


**TIMING DIAGRAM FOR IP COUNTER**
![Screenshot 2024-12-18 141610](https://github.com/user-attachments/assets/4af1fa9d-1857-464e-b55e-d3d57270fb33)


**TRUTH TABLE**
![329800972-a8caffaa-dc40-44b8-82dd-a12604a04eaa](https://github.com/user-attachments/assets/9f9b3900-88e4-4223-a4d7-31bad5b4cd55)

**RESULTS**

 Thus the program executed successfully.
