# LAB 3

## First task

### Table with connection of 16 slide switches and 16 LEDs on Nexys A7 board

| **Switch** | **Resistor [Ω]** | **Pin** | 
| :-: | :-: | :-: |
| SW0 | 10K | J15 |
| SW1 | 10K | L16 |
| SW2 | 10K | M13 | 
| SW3 | 10K | R15 | 
| SW4 | 10K | R17 |
| SW5 | 10K | T18 | 
| SW6 | 10K | U18 | 
| SW7 | 10K | R13 | 
| SW8 | 10K | T8 | 
| SW9 | 10K | U8 | 
| SW10 | 10K | R16 | 
| SW11 | 10K | T13 | 
| SW12 | 10K | H6 | 
| SW13 | 10K | U12 | 
| SW14 | 10K | U11 | 
| SW15 | 10K | V10 | 

| **LED diode** | **Resistor [Ω]** | **Pin** | **Active** | 
| :-: | :-: | :-: | :-: |
| LED0 | 330 | H17 | High |
| LED1 | 330 | K15 | High |
| LED2 | 330 | J13 | High |
| LED3 | 330 | N14 | High |
| LED4 | 330 | R18 | High |
| LED5 | 330 | V17 | High |
| LED6 | 330 | U17 | High |
| LED7 | 330 | U16 | High |
| LED8 | 330 | V16 | High |
| LED9 | 330 | T15 | High |
| LED10 | 330 | U14 | High |
| LED11 | 330 | T16 | High |
| LED12 | 330 | V15 | High |
| LED13 | 330 | V14 | High |
| LED14 | 330 | V12 | High |
| LED15 | 330 | V11 | High |

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
