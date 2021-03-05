# LAB 4

## First task

### Table with connection of 7-segment displays on Nexys A7 board

| **Component** | **Resistor [Ω]** | **PIN** | 
| :-: | :-: | :-: |
| AN0 | 2K2 | J17 | 
| AN1 | 2K2 | J18 | 
| AN2 | 2K2 | T9 | 
| AN3 | 2K2 | J14 | 
| AN4 | 2K2 | P14 | 
| AN5 | 2K2 | T14 | 
| AN6 | 2K2 | K2 | 
| AN7 | 2K2 | U13 | 
| CA | 100 | T10 |
| CB | 100 | R10 |
| CC | 100 | K16 | 
| CD | 100 | K13 | 
| CE | 100 | P15 |
| CF | 100 | T11 | 
| CG | 100 | L18 | 
| DP | 100 | H15 | 

### Decoder truth table for common anode 7-segment display

| **Hex** | **Inputs** | **A** | **B** | **C** | **D** | **E** | **F** | **G** |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| 0 | 0000 | 0 | 0 | 0 | 0 | 0 | 0 | 1 |
| 1 | 0001 | 1 | 0 | 0 | 1 | 1 | 1 | 1 |
| 2 | 0010 | 0 | 0 | 1 | 0 | 0 | 1 | 0 |
| 3 | 0011 | 0 | 0 | 0 | 0 | 1 | 1 | 0 |
| 4 | 0100 | 1 | 0 | 0 | 1 | 1 | 0 | 0 |
| 5 | 0101 | 0 | 1 | 0 | 0 | 1 | 0 | 0 |
| 6 | 0110 | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
| 7 | 0111 | 0 | 0 | 0 | 1 | 1 | 1 | 1 |
| 8 | 1000 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 9 | 1001 | 0 | 0 | 0 | 0 | 1 | 0 | 0 |
| A | 1010 | 0 | 0 | 0 | 1 | 0 | 0 | 0 |
| b | 1011 | 1 | 1 | 0 | 0 | 0 | 0 | 0 |
| C | 1100 | 0 | 1 | 1 | 0 | 0 | 0 | 1 |
| d | 1101 | 1 | 0 | 0 | 0 | 0 | 1 | 0 |
| E | 1110 | 0 | 1 | 1 | 0 | 0 | 0 | 0 |
| F | 1111 | 0 | 1 | 1 | 1 | 0 | 0 | 0 |

## Second task

### VHDL architecture from source file mux_2bit_4to1.vhd

```vhdl
architecture Behavioral of mux_2bit_4to1 is
begin
    f_o <=  a_i when (sel_i = "00") else
            b_i when (sel_i = "01") else
            c_i when (sel_i = "10") else
            d_i;
end architecture Behavioral;
```

### VHDL stimulus process from testbench file tb_mux_2bit_4to1.vhd

```vhdl
p_stimulus : process
    begin
        -- Report a note at the begining of stimulus process
        report "Stimulus process started" severity note;

        -- First test values
        s_d <= "00"; s_c <= "11"; s_b <= "11"; s_a <= "11";
        s_sel <= "00";
        wait for 100 ns;
        
        s_d <= "10"; s_c <= "01"; s_b <= "01"; s_a <= "00";
        s_sel <= "00";
        wait for 100 ns;
        
        s_d <= "10"; s_c <= "01"; s_b <= "01"; s_a <= "11";
        s_sel <= "01";
        wait for 100 ns;
        
        s_d <= "10"; s_c <= "01"; s_b <= "11"; s_a <= "00";
        s_sel <= "01";
        wait for 100 ns;
        
        s_sel <= "10";
        wait for 100 ns;
        
        s_sel <= "11";
        wait for 100 ns;

        -- Report a note at the end of stimulus process
        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;
```

### Screenshot of simulated time waveforms

![simulated time waveforms](Images/waveforms.JPG)

## Third task

### Tutorial for Vivado design flow

#### Project creation
   - File -> Project -> New
        - Next
   - Name project and modify project location
        - Next
   - Choose **RTL Project**
        - Next
   - Add or create source file (type: VHDL) and name it
        - Next
   - Add or create constraint file (optional)
        - Next
   - Choose from a Board or part you'll be working with ( ex. Nexys A7-50T)
        - Next
   - Finish

#### Adding source file
   - File -> Add sources
   - Add or create design sources
        - Next
   - Add or create source file (type: VHDL) and name it
   - Finish
   - Define module and specify sources (optional)
   - Ok and confirm

#### Adding testbench file
   - File -> Add sources
   - Add or create simulation sources
        - Next
   - Add or create source file (type: VHDL) and name it
   - Finish
   - Define module and specify sources (optional)
   - Ok and confirm

#### Running simulation
   - Flow -> Run simulation -> Run behavioral simulation
   - To change value form (binary, etc) or color of its waveform, right-click on its name or value and select radix / Signal color
   - To close simulation close the blue popup line above the main window (**Simulation**-Behavioral simulation-Name_of_the_simulation_Name_of_the_testbench )

#### Adding XDC constraints file
   - File -> Add sources
   - Add or create Constraints
        - Next
   - Add or create source file (type: XDC) and name it
   - Finish
