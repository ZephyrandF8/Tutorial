Seccion 1
<img width="1373" height="550" alt="image" src="https://github.com/user-attachments/assets/6c692a73-2218-495a-bf1d-becb03e0dcf7" />
<img width="1000" height="514" alt="Zdig" src="https://github.com/user-attachments/assets/abb2cf4c-b0d5-4707-b470-445ae4d92048" />
Seccion 2
<img width="1379" height="1006" alt="make wv" src="https://github.com/user-attachments/assets/79c6a4b7-8f3b-4772-9ca5-1533811b0cf4" />
<img width="1918" height="1018" alt="make wv lcd_spi" src="https://github.com/user-attachments/assets/af74d6ae-94aa-47f9-8668-baf0a09ea35e" />
<img width="1093" height="372" alt="Make test" src="https://github.com/user-attachments/assets/b73237f1-826a-419b-aac0-33e382dcf553" />
<img width="1079" height="341" alt="make test en lcd_spi" src="https://github.com/user-attachments/assets/21f14939-8556-4f62-ad74-77df382808c6" />
<img width="948" height="176" alt="make synth" src="https://github.com/user-attachments/assets/b0042787-09c0-4577-9fa1-691983c8180f" />
<img width="1125" height="245" alt="make synth y pnr led" src="https://github.com/user-attachments/assets/fa9eab9d-fa01-4c60-bdbf-f778dda99f77" />
![Imagen de WhatsApp 2025-08-14 a las 19 37 14_8728d176](https://github.com/user-attachments/assets/378b28a9-7117-46b3-b596-5fe724dc5c1c)


Seccion 3
![Funcionamiento2](https://github.com/user-attachments/assets/99c7f940-47f5-4f7a-b40b-4e1fa738dbe6)
![Funcionamiento1](https://github.com/user-attachments/assets/b5865e86-bd78-4c03-acd2-5ef4eb982a3e)
<img width="1255" height="859" alt="Make synth" src="https://github.com/user-attachments/assets/7f054505-d30e-461c-a695-3b6b5c504240" />
<img width="1214" height="294" alt="Make bitstream make load" src="https://github.com/user-attachments/assets/5c901251-0ac0-4db7-adac-fee6f92f80b9" />


Design
module module_counter # ( 
    parameter COUNT= 13500000 //contador parametrizable que cuenta hasta 13500 000 , ese valor es porque el clk es de 27Mhz, por lo que cada segundo llega a ese numero apox
)(
    input logic clk,
    input logic rst, 

    output logic [5 : 0] count_o //[de: hasta] // y el nombre que tendra esa cuenta de bits  
);
    localparam WIDTH_COUNT =$clog2(COUNT);  // me dice cuantos bits necesito para representar el count 
    logic[WIDTH_COUNT - 1: 0] clk_counter = '0; //variable tipo logic que cuenta de width-1 a 0 cuyo nombre es clk_counter y se inizializa en 0, el apostrofe dice que all bits van a 0
    logic [5 : 0] led_count_r; // una varable registrada  que seran flipflops para almacenar los valores de  cada bit

    always_ff @(posedge clk) begin //always ff viene a decir que hay un flip flop y que en el flanco positivo de reloj haga algo
        if (!rst) begin   // la !rst viene a indicar negacion y que se activa en bajo
            led_count_r <= '0;
            clk_counter <= '0;
        end 
        else if (clk_counter==COUNT-1) begin //esto hace que cuando lleguemos a 13500 000 se reinicie el clk, al haber contado un segundo
            clk_counter <= '0;
            led_count_r <= led_count_r + 1'b1; //aumenta en 1 la representacion de la cuenta, que es visual a traves de las luces
        end 
        else begin
            clk_counter <= clk_counter + 1'b1; //Si no llegamos a la cuenta maxima y tampoco se resetea, el sigue contando añadiendo =1 
            led_count_r <= led_count_r; // Pero la cuenta se mantiene igual
        end
    end

    assign count_o = ~led_count_r;   //La cuenta que va a los es la cuenta de leds negados

endmodule 


Testbench
`timescale 1ns/1ps    

module module_counter_tb;

    logic clk;                  // Los estimulos
    logic rst;
    logic [5 : 0 ] count_o;

    module_counter # (10)  COUNTER( //La instancia del modulo 
        .clk (clk),
        .rst (rst), 
        .count_o(count_o)
    );

    initial begin //Inciar la simulacion
        clk = 0;
        rst = 1;

        #30; //Despues de 30unidades de tiempo, 30nS

        rst = 0; //Resetamos
        #30;      

        rst=1;    //dejamos de resetear

        # 300000;  
        $finish;    //fonalizamos la simulacion
    end
    always begin
        clk = ~clk;
        #10;       
    end

    initial begin
        $dumpfile("module_counter_tb.vcd");
        $dumpvars(0, module_counter_tb);  //0, nombre del modulo 
    end
    
endmodule


Constrains
<img width="1116" height="510" alt="image" src="https://github.com/user-attachments/assets/84fb7110-1e94-4801-9aa7-fff2857c92c9" />
