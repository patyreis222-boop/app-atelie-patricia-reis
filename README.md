<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ateliê Patrícia Reis</title>

<style>
body{
    font-family: Arial, sans-serif;
    background:#ffffff;
    margin:0;
    padding:20px;
}
.container{
    max-width:500px;
    margin:auto;
}
h1{
    text-align:center;
    color:#4A6FA5;
}
input, select{
    width:100%;
    padding:12px;
    margin:6px 0 15px 0;
    border-radius:8px;
    border:1px solid #ccc;
    font-size:16px;
}
button{
    width:100%;
    padding:14px;
    background:#4A6FA5;
    color:white;
    border:none;
    border-radius:8px;
    font-size:16px;
}
.result{
    margin-top:20px;
    padding:15px;
    background:#f3f6fb;
    border-radius:10px;
    font-size:15px;
}
@media print{
    button{
        display:none;
    }
}
</style>
</head>

<body>

<div class="container">

<h1>Orçamento</h1>

<label>Nome do Cliente</label>
<input type="text" id="cliente">

<label>Largura (m)</label>
<input type="number" id="largura" step="0.01">

<label>Altura (m)</label>
<input type="number" id="altura" step="0.01">

<label>Modelo</label>
<select id="modelo">
<option>Prega Macho</option>
<option>Wave</option>
<option>Americana</option>
<option>Ilhós</option>
</select>

<label>Tipo</label>
<select id="tipo">
<option value="sem">Sem Forro</option>
<option value="com">Com Forro</option>
</select>

<label>Valor Metro Linear Sem Forro</label>
<input type="number" id="mlSem" value="150">

<label>Valor Metro Linear Com Forro</label>
<input type="number" id="mlCom" value="200">

<label>Lucro Desejado (R$)</label>
<input type="number" id="lucro">

<button onclick="calcular()">Calcular</button>

<div class="result" id="resultado"></div>

<button onclick="window.print()">Gerar PDF</button>

</div>

<script>
function calcular(){

let largura = parseFloat(document.getElementById('largura').value) || 0;
let tipo = document.getElementById('tipo').value;
let mlSem = parseFloat(document.getElementById('mlSem').value) || 0;
let mlCom = parseFloat(document.getElementById('mlCom').value) || 0;
let lucro = parseFloat(document.getElementById('lucro').value) || 0;

let consumo = (largura * 3.5) / 3;
let rodizios = Math.round((largura * 100) / 10);
let trilho = largura * 16;

let valorMetro = tipo === "sem" ? mlSem : mlCom;
let confeccao = largura * valorMetro;

let custoBase = confeccao + trilho;
let valorFinal = custoBase + lucro;

document.getElementById("resultado").innerHTML =
"<strong>Consumo de Tecido:</strong> " + consumo.toFixed(2) + " m<br>" +
"<strong>Rodízios:</strong> " + rodizios + " un<br>" +
"<strong>Trilho:</strong> R$ " + trilho.toFixed(2) + "<br>" +
"<strong>Confecção:</strong> R$ " + confeccao.toFixed(2) + "<br><br>" +
"<strong>Custo Base:</strong> R$ " + custoBase.toFixed(2) + "<br>" +
"<strong>Lucro:</strong> R$ " + lucro.toFixed(2) + "<br><br>" +
"<strong style='font-size:18px;'>VALOR FINAL:</strong> R$ " + valorFinal.toFixed(2);

}
</script>

</body>
</html>
