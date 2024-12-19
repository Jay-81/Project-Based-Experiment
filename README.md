### Project-Based-Experiment

**AIM:**

To design and simulate the traffic light controller.

**SOFTWATE USED:**

Quartus II

**THEORY:**
	
     	Consider a controller for traffic at the intersection of three main roads.  

  ![image](https://github.com/naavaneetha/Project-Based-Experiment/assets/154305477/e3af03dd-a4de-4b21-af0a-a5a332a3e4b6)


 The traffic signal for all the three main roads have equal priority and they remain red by default.

 In state 00,the traffic signals remains red for first five counts and yellow of road1 turns on for the next four counts.

 In state 01, the green of road1 turns on for first five counts and yellow of road1 and road2 turns on for next four4 counts of this state.
 
 In state 10, the traffic signal of road1 comes back to the red and that of road2 goes to green for tee first five counts.For the next four counts the traffic signal of road2 and road3 remains yellow.


 In state 11, the traffic signal of road2 comes back to the red and that of road3 goes to green for the first five counts.For the next four counts the traffic signal of road3 turns to yellow

 At the end of four states,the traffic signal of all the three roads come back to red.

**Task Assigned**

From the HDL code given formulate the correct code  to divert the traffic to path 1 direction and disable the control in other directions (Assume user is at MR3 spot)

**Procedure**

1.	Type the program in Quartus software.
2.	Compile and run the program.
3.	Generate the RTL schematic and save the logic diagram.
4.	Create nodes for inputs and outputs to generate the timing diagram.
5.	For different input combinations generate the timing diagram
   
**Program:**

/* Program to implement the given logic function and to verify its operations in quartus using Verilog programming. 

Developed by: JAYANI K
RegisterNumber: 24005008
*/
~~~
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_ARITH.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;

entity traffic_controller is
    Port ( clk      : in  STD_LOGIC;
           reset    : in  STD_LOGIC;
           red1     : out STD_LOGIC;
           yellow1  : out STD_LOGIC;
           green1   : out STD_LOGIC;
           red2     : out STD_LOGIC;
           yellow2  : out STD_LOGIC;
           green2   : out STD_LOGIC;
           red3     : out STD_LOGIC;
           yellow3  : out STD_LOGIC;
           green3   : out STD_LOGIC);
end traffic_controller;

architecture Behavioral of traffic_controller is
    type state_type is (state00, state01, state10, state11);
    signal state     : state_type := state00;
    signal counter   : integer := 0;
begin
    process(clk, reset)
    begin
        if reset = '1' then
            state <= state00;
            counter <= 0;
        elsif rising_edge(clk) then
            counter <= counter + 1;
            if counter = 9 then
                counter <= 0;
                case state is
                    when state00 =>
                        state <= state01;
                    when state01 =>
                        state <= state10;
                    when state10 =>
                        state <= state11;
                    when state11 =>
                        state <= state00;
                end case;
            end if;
        end if;
    end process;

    process(state, counter)
    begin
        -- Default all signals to red
        red1 <= '1'; yellow1 <= '0'; green1 <= '0';
        red2 <= '1'; yellow2 <= '0'; green2 <= '0';
        red3 <= '1'; yellow3 <= '0'; green3 <= '0';

        case state is
            when state00 =>
                if counter < 5 then
                    red1 <= '1'; yellow1 <= '0'; green1 <= '0';
                else
                    red1 <= '0'; yellow1 <= '1'; green1 <= '0';
                end if;
            when state01 =>
                if counter < 5 then
                    red1 <= '0'; yellow1 <= '0'; green1 <= '1';
                else
                    red1 <= '0'; yellow1 <= '1'; green1 <= '0';
                    yellow2 <= '1';
                end if;
            when state10 =>
                if counter < 5 then
                    red2 <= '0'; yellow2 <= '0'; green2 <= '1';
                else
                    red2 <= '0'; yellow2 <= '1'; green2 <= '0';
                    yellow3 <= '1';
                end if;
            when state11 =>
                if counter < 5 then
                    red3 <= '0'; yellow3 <= '0'; green3 <= '1';
                else
                    red3 <= '0'; yellow3 <= '1'; green3 <= '0';
                end if;
        end case;
    end process;
end Behavioral;
~~~

**RTL Schematic**

![image](https://github.com/user-attachments/assets/b8366b9f-5e2c-4ea4-8402-fffb2f45b9dd)

**Output Timing Waveform**
![331813847-099d62f4-f190-4802-8082-48ae0812360b](https://github.com/user-attachments/assets/b9234789-96d1-41f8-9f51-9ccca2fac921)

**Result:**

Thus the HDL program was executed successfully
