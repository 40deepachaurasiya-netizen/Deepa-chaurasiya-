module pipeline_reg (
    input  logic        clk,
    input  logic        rst_n,

    input  logic        in_valid,
    output logic        in_ready,
    input  logic [31:0] in_data,

    output logic        out_valid,
    input  logic        out_ready,
    output logic [31:0] out_data
);

    logic [31:0] data;
    logic        valid;

    assign in_ready  = (!valid) || out_ready;
    assign out_valid = valid;
    assign out_data  = data;

    always_ff @(posedge clk or negedge rst_n) begin
        if (!rst_n) begin
            valid <= 1'b0;
            data  <= 32'd0;
        end
        else begin
            if (in_valid && in_ready) begin
                data  <= in_data;
                valid <= 1'b1;
            end
            else if (out_valid && out_ready) begin
                valid <= 1'b0;
            end
        end
    end

endmodule# Deepa-chaurasiya-
