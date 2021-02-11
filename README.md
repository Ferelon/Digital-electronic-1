# Digital-electronic-1

## Jiří Kadlec

[VUT profile](https://www.vutbr.cz/lide/jiri-kadlec-223381) [LinkedIn](https://www.linkedin.com/in/jkadlec01001000x01100101x01101100x01101100x01101111x00100001/)

**VUT number: 223381**   
*ID: xkadle41*

### Hello world VHDL

```vhdl
entity T01_HelloWorldTb is
end entity;
architecture sim of T01_HelloWorldTb is
begin
    process is
    begin
        report "Hello World!";
        wait;
    end process;
end architecture;
```
### VHDL operators

| **Operator** | **Description** |
| :-: | :-- |
| `<=` | Value assignment |
| `and` | Logical AND |
| `nand` | Logical AND with negated output |
| `or` | Logical OR |
| `nor` | Logical OR with negated output |
| `not` | Nagation |
| `xor` | Exclusive OR |
| `xnor` | Exclusive OR with negated output |
| `-- comment` | Comments |
