# LAB 7

## First task

### Timing diagram figure for displaying value 3.142

   | **D** | **Qn** | **Q(n+1)** | **Comments** |
   | :-: | :-: | :-: | :-- |
   | 0 | 0 | 0 | No change |
   | 0 | 1 | 0 | Reset |
   | 1 | 0 | 1 | No change |
   | 1 | 1 | 1 | Set |

   | **J** | **K** | **Qn** | **Q(n+1)** | **Comments** |
   | :-: | :-: | :-: | :-: | :-- |
   | 0 | 0 | 0 | 0 | No change |
   | 0 | 0 | 1 | 1 | No change |
   | 0 | 1 | 0 | 0 | Reset |
   | 0 | 1 | 1 | 0 | Reset |
   | 1 | 0 | 0 | 1 | Set |
   | 1 | 0 | 1 | 1 | Set |
   | 1 | 1 | 0 | 1 | Toggle |
   | 1 | 1 | 1 | 0 | Toggle |

   | **T** | **Qn** | **Q(n+1)** | **Comments** |
   | :-: | :-: | :-: | :-- |
   | 0 | 0 | 0 | No change |
   | 0 | 1 | 1 | No change |
   | 1 | 0 | 1 | Invert (toggle) |
   | 1 | 1 | 0 | Invert (toggle) |

## Second task

### VHDL code listing of the process p_d_latch with syntax highlighting

```vhdl
    p_d_latch : process(en, d, arst)
    begin
        if (arst = '1') then
            q     <= '0';
            q_bar <= '1';
        elsif (en = '1') then
            q     <= d;
            q_bar <= not d;
        end if;
    end process p_d_latch;
```

### Listing of VHDL reset and stimulus processes from the testbench tb_d_latch file with syntax highlighting and asserts

```vhdl
    p_stimulus : process
    begin
        report "Stimulus process started" severity note;

        s_arst <= '0';
        s_en   <= '0'; 
        s_d    <= '0'; wait for 50 ns;
        s_d    <= '1'; wait for 50 ns;
        s_d    <= '0'; wait for 50 ns;
        s_d    <= '1'; wait for 50 ns;
        
        s_en   <= '1'; 
        s_d    <= '0'; wait for 50 ns;
        s_d    <= '1'; wait for 50 ns;
        s_d    <= '0'; wait for 50 ns;
        s_d    <= '1'; wait for 50 ns;
        
        s_arst <= '1';
        s_en   <= '0'; 
        s_d    <= '1'; wait for 50 ns;
        s_d    <= '0'; wait for 50 ns;
        s_d    <= '0'; wait for 50 ns;
        s_d    <= '1'; wait for 50 ns;
        
        s_en   <= '1';
        s_d    <= '1'; wait for 50 ns;
        s_d    <= '0'; wait for 50 ns;
        s_d    <= '0'; wait for 50 ns;
        s_d    <= '1'; wait for 50 ns;

        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;
```

### Screenshot with simulated time waveforms; always display all inputs and outputs

![simulated time waveforms](Images/waveform.JPG)

## Third task

### VHDL code listing of the processes p_d_ff_arst, p_d_ff_rst, p_jk_ff_rst, p_t_ff_rst with syntax highlighting

#### p_d_ff_arst

```vhdl
    p_d_ff_arst : process(clk, arst)
    begin
        if (arst = '1') then
            q     <= '0';
            q_bar <= '1';
        elsif rising_edge(clk) then
            q     <= d;
            q_bar <= not d;
        end if;
    end process p_d_ff_arst;
```

#### p_d_ff_rst

```vhdl
    p_d_ff_rst : process(clk)
    begin
        if rising_edge(clk) then
            if (rst = '1') then
                q     <= '0';
                q_bar <= '1';
            else
                q     <= d;
                q_bar <= not d;
            end if;
        end if;
    end process p_d_ff_rst;
```

#### p_jk_ff_rst

```vhdl
    p_jk_ff_rst : process(clk)
    begin
        if rising_edge(clk) then
            if (rst = '1') then
                s_q     <= '0';
                s_q_bar <= '1';
            else
                if (j = '0' and k = '0') then
                    s_q     <= s_q;
                    s_q_bar <= s_q_bar;
                elsif (j = '0' and k = '1') then
                    s_q     <= '0';
                    s_q_bar <= '1';
                elsif (j = '1' and k = '0') then
                    s_q     <= '1';
                    s_q_bar <= '0';
                else
                    s_q     <= not s_q;
                    s_q_bar <= not s_q_bar;
                end if;
            end if;
        end if;
    end process p_jk_ff_rst;
    q     <= s_q;
    q_bar <= s_q_bar;
```

#### p_t_ff_rst

```vhdl
    p_t_ff_rst : process(clk)
    begin
        if rising_edge(clk) then
            if (rst = '1') then
                s_q     <= '0';
                s_q_bar <= '1';
            elsif (t = '1') then
                s_q     <= not s_q;
                s_q_bar <= s_q;
            end if;
        end if;
    end process p_t_ff_rst;
    q     <= s_q;
    q_bar <= s_q_bar;
```

### Listing of VHDL clock, reset and stimulus processes from the testbench files with syntax highlighting and asserts

#### p_d_ff_arst

```vhdl
    p_clk_gen : process
    begin
        while now < 750 ns loop
            s_clk <= '0';
            wait for c_CLK_100MHZ_PERIOD / 2;
            s_clk <= '1';
            wait for c_CLK_100MHZ_PERIOD / 2;
        end loop;
        wait;
    end process p_clk_gen;

    p_reset_gen : process
    begin
        s_arst <= '0'; wait for 50 ns;
        s_arst <= '1'; wait for 50 ns;
        s_arst <= '0'; 
        wait;
    end process p_reset_gen;

    p_stimulus : process
    begin
        report "Stimulus process started" severity note;
        s_d    <= '1'; wait for 20 ns;
        s_d    <= '0'; wait for 20 ns;
        s_d    <= '1'; wait for 20 ns;
        s_d    <= '0'; wait for 20 ns;
        s_d    <= '1'; wait for 20 ns;
        s_d    <= '0'; wait for 20 ns;
        s_d    <= '1'; wait for 20 ns;
        s_d    <= '0'; wait for 20 ns;
        s_d    <= '1'; wait for 20 ns;
        s_d    <= '0'; wait for 20 ns;
        s_d    <= '1'; wait for 20 ns;
        s_d    <= '0'; wait for 20 ns;
        s_d    <= '1'; wait for 20 ns;
        s_d    <= '0'; wait for 20 ns;
        s_d    <= '1'; wait for 20 ns;
        s_d    <= '0'; wait for 20 ns;
        s_d    <= '1'; wait for 20 ns;
        s_d    <= '0'; wait for 20 ns;
        s_d    <= '1'; wait for 20 ns;
        s_d    <= '0'; wait for 20 ns;
        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;
```

#### p_d_ff_rst

```vhdl
    p_clk_gen : process
    begin
        while now < 750 ns loop
            s_clk <= '0';
            wait for c_CLK_100MHZ_PERIOD / 2;
            s_clk <= '1';
            wait for c_CLK_100MHZ_PERIOD / 2;
        end loop;
        wait;
    end process p_clk_gen;

    p_reset_gen : process
    begin
        s_rst <= '0'; wait for 120 ns;
        s_rst <= '1'; wait for 110 ns;
        s_rst <= '0'; 
        wait;
    end process p_reset_gen;

    p_stimulus : process
    begin
        report "Stimulus process started" severity note;
        s_d <= '0'; wait for 20 ns;
        s_d <= '1'; wait for 20 ns;
        s_d <= '0'; wait for 20 ns;
        s_d <= '1'; wait for 20 ns;
        s_d <= '0'; wait for 20 ns;
        s_d <= '1'; wait for 20 ns;
        s_d <= '0'; wait for 20 ns;
        s_d <= '1'; wait for 20 ns;
        s_d <= '0'; wait for 20 ns;
        s_d <= '1'; wait for 20 ns;
        s_d <= '0'; wait for 20 ns;
        s_d <= '1'; wait for 20 ns;
        s_d <= '0'; wait for 20 ns;
        s_d <= '1'; wait for 20 ns;
        s_d <= '0'; wait for 20 ns;
        s_d <= '1'; wait for 20 ns;
        s_d <= '0'; wait for 20 ns;
        s_d <= '1'; wait for 20 ns;
        s_d <= '0'; wait for 20 ns;
        s_d <= '1'; wait for 20 ns;
        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;
```

#### p_jk_ff_rst

```vhdl
    p_clk_gen : process
    begin
        while now < 750 ns loop
            s_clk <= '0';
            wait for c_CLK_100MHZ_PERIOD / 2;
            s_clk <= '1';
            wait for c_CLK_100MHZ_PERIOD / 2;
        end loop;
        wait;
    end process p_clk_gen;

    p_reset_gen : process
    begin
        s_rst <= '1'; wait for 10 ns;
        s_rst <= '0'; wait for 100 ns;
        s_rst <= '1'; wait for 100 ns;
        s_rst <= '0'; 
        wait;
    end process p_reset_gen;

    p_stimulus : process
    begin
        report "Stimulus process started" severity note;
        s_j <= '0'; s_k <= '0'; wait for 20 ns;
        s_j <= '0'; s_k <= '1'; wait for 20 ns;
        s_j <= '1'; s_k <= '0'; wait for 20 ns;
        s_j <= '1'; s_k <= '1'; wait for 20 ns;
        s_j <= '0'; s_k <= '0'; wait for 20 ns;
        s_j <= '0'; s_k <= '1'; wait for 20 ns;
        s_j <= '1'; s_k <= '0'; wait for 20 ns;
        s_j <= '1'; s_k <= '1'; wait for 20 ns;
        s_j <= '0'; s_k <= '0'; wait for 20 ns;
        s_j <= '0'; s_k <= '1'; wait for 20 ns;
        s_j <= '1'; s_k <= '0'; wait for 20 ns;
        s_j <= '1'; s_k <= '1'; wait for 20 ns;
        s_j <= '0'; s_k <= '0'; wait for 20 ns;
        s_j <= '0'; s_k <= '1'; wait for 20 ns;
        s_j <= '1'; s_k <= '0'; wait for 20 ns;
        s_j <= '1'; s_k <= '1'; wait for 20 ns;
        s_j <= '0'; s_k <= '0'; wait for 20 ns;
        s_j <= '0'; s_k <= '1'; wait for 20 ns;
        s_j <= '1'; s_k <= '0'; wait for 20 ns;
        s_j <= '1'; s_k <= '1'; wait for 20 ns;
        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;
```

#### p_t_ff_rst

```vhdl
    p_clk_gen : process
    begin
        while now < 750 ns loop
            s_clk <= '0';
            wait for c_CLK_100MHZ_PERIOD / 2;
            s_clk <= '1';
            wait for c_CLK_100MHZ_PERIOD / 2;
        end loop;
        wait;
    end process p_clk_gen;

    p_reset_gen : process
    begin
        s_rst <= '1'; wait for 10 ns;
        s_rst <= '0'; wait for 100 ns;
        s_rst <= '1'; wait for 100 ns;
        s_rst <= '0'; 
        wait;
    end process p_reset_gen;

    p_stimulus : process
    begin
        report "Stimulus process started" severity note;
        s_t <= '0'; wait for 20 ns;
        s_t <= '1'; wait for 20 ns;
        s_t <= '0'; wait for 20 ns;
        s_t <= '1'; wait for 20 ns;
        s_t <= '0'; wait for 20 ns;
        s_t <= '1'; wait for 20 ns;
        s_t <= '0'; wait for 20 ns;
        s_t <= '1'; wait for 20 ns;
        s_t <= '0'; wait for 20 ns;
        s_t <= '1'; wait for 20 ns;
        s_t <= '0'; wait for 20 ns;
        s_t <= '1'; wait for 20 ns;
        s_t <= '0'; wait for 20 ns;
        s_t <= '1'; wait for 20 ns;
        s_t <= '0'; wait for 20 ns;
        s_t <= '1'; wait for 20 ns;
        s_t <= '0'; wait for 20 ns;
        s_t <= '1'; wait for 20 ns;
        s_t <= '0'; wait for 20 ns;
        s_t <= '1'; wait for 20 ns;
        report "Stimulus process finished" severity note;
        wait;
    end process p_stimulus;
```

### Screenshot, with simulated time waveforms; always display all inputs and outputs

#### p_d_ff_arst

![simulated time waveforms](Images/waveform0.JPG)

#### p_d_ff_rst

![simulated time waveforms](Images/waveform1.JPG)

#### p_jk_ff_rst

![simulated time waveforms](Images/waveform2.JPG)

#### p_t_ff_rst

![simulated time waveforms](Images/waveform3.JPG)

## Fourth task

### Image of the shift register schematic

![schema of shift register](Images/schema.JPG)
