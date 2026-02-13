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
    max-width:600px;
    margin:auto;
}
h1{
    text-align:center;
    color:#3b5fa0;
}
input, select{
    width:100%;
    padding:10px;
    margin:6px 0 15px 0;
    border-radius:8px;
    border:1px solid #ccc;
}
button{
    width:100%;
    padding:12px;
    background:#3b5fa0;
    color:white;
    border:none;
    border-radius:8px;
    font-size:16px;
}
.result{
    margin-top:20px;
    padding:15px;
    background:#f4f6fb;
    border-radius:10px;
}
@media print{
    .no-print{
        display:none;
    }
}
.pdf-header{
    text-align:center;
    border-bottom:2px solid #3b5fa0;
    margin-bottom:20px;
    padding-bottom:10px;
}
.valor-final{
    font-size:22px;
    font-weight:bold;
    color:#3b5fa0;
}
</style>
</head>

<body>

<div class="container" id="app">

<h1>Orçamento</h1>

<div class="no-print">

<label>Nome do Cliente</label>
<input type="text" id="cliente">

<label>Largura (m)</label>
<input type="number" id="largura" step="0.01">

<label>Altura (m)</label>
<input type="number" id="altura" step="0.01">

<label>Acabamento Superior (cm)</label>
<input type="number" id="acabamento" value="12">

<label>Barra (cm)</label>
<input type="number" id="barra" value="32">

<label>Tipo de Forro</label>
<select id="tipo">
<option value="Sem Forro">Sem Forro</option>
<option value="Microfibra">Microfibra</option>
<option value="Blackout 70%">Blackout 70%</option>
<option value="Blackout 100%">Blackout 100%</option>
</select>

<label>Tipo de Trilho</label>
<select id="tipoTrilho">
<option value="teto">Trilho de Teto</option>
<option value="varao">Varão Suíço</option>
</select>

<label>Valor do Varão Suíço (R$/m)</label>
<input type="number" id="valorVarao">

<label>Quantidade de Presilhas</label>
<input type="number" id="presilhas" value="7">

<label>Instalação (R$)</label>
<input type="number" id="instalacao">

<label>Desconto (R$)</label>
<input type="number" id="desconto">

<button onclick="calcular()">Calcular</button>

</div>

<div class="result" id="resultado"></div>

<button onclick="gerarPDF()">Gerar PDF Cliente</button>

</div>

<script>

function calcular(){

let largura = parseFloat(document.getElementById("largura").value) || 0;
let altura = parseFloat(document.getElementById("altura").value) || 0;
let acabamento = (parseFloat(document.getElementById("acabamento").value) || 0)/100;
let barra = (parseFloat(document.getElementById("barra").value) || 0)/100;
let tipo = document.getElementById("tipo").value;
let tipoTrilho = document.getElementById("tipoTrilho").value;
let valorVarao = parseFloat(document.getElementById("valorVarao").value) || 0;
let presilhas = parseFloat(document.getElementById("presilhas").value) || 0;
let instalacao = parseFloat(document.getElementById("instalacao").value) || 0;
let desconto = parseFloat(document.getElementById("desconto").value) || 0;

let precoGaze = 27;
let precoMicrofibra = 15;
let precoBlackout70 = 25;
let precoBlackout100 = 33;
let precoEntretela = 2;
let precoRodizio = 0.15;
let precoTrilhoTeto = 16;

let alturaFinal = altura + acabamento + barra;

// Gaze
let alturasGaze = Math.ceil((largura * 3.5) / 3);
let metragemGaze = alturasGaze * alturaFinal;
let custoGaze = metragemGaze * precoGaze;
let custoEntretela = metragemGaze * precoEntretela;

// Forro
let alturasForro = 0;
let metragemForro = 0;
let custoForro = 0;

if(tipo !== "Sem Forro"){
    alturasForro = Math.ceil((largura * 2) / 3);
    metragemForro = alturasForro * alturaFinal;

    if(tipo === "Microfibra") custoForro = metragemForro * precoMicrofibra;
    if(tipo === "Blackout 70%") custoForro = metragemForro * precoBlackout70;
    if(tipo === "Blackout 100%") custoForro = metragemForro * precoBlackout100;
}

// Rodízios
let rodizios = Math.ceil(largura * 10);
let custoRodizios = rodizios * precoRodizio;

// Trilho
let custoTrilho = 0;
if(tipoTrilho === "teto"){
    custoTrilho = largura * precoTrilhoTeto;
}else{
    custoTrilho = largura * valorVarao;
}

// Mão de obra (venda)
let valorVenda = 0;
if(tipo === "Sem Forro"){
    valorVenda = largura * 150;
}else{
    valorVenda = largura * 200;
}

let custoTotal = custoGaze + custoEntretela + custoForro + custoRodizios + (presilhas*2) + custoTrilho;
let valorFinal = valorVenda + instalacao - desconto;
let lucro = valorFinal - custoTotal;

document.getElementById("resultado").innerHTML = `
<b>Altura Final:</b> ${alturaFinal.toFixed(2)} m<br><br>

<b>GAZE:</b> ${alturasGaze} alturas - ${metragemGaze.toFixed(2)} m<br>
<b>FORRO:</b> ${alturasForro} alturas - ${metragemForro.toFixed(2)} m<br><br>

<b>Custo Total:</b> R$ ${custoTotal.toFixed(2)}<br>
<b>Valor de Venda:</b> R$ ${valorVenda.toFixed(2)}<br>
<b>Instalação:</b> R$ ${instalacao.toFixed(2)}<br>
<b>Desconto:</b> R$ ${desconto.toFixed(2)}<br><br>

<b class="valor-final">VALOR FINAL: R$ ${valorFinal.toFixed(2)}</b><br><br>

<b>Lucro Estimado:</b> R$ ${lucro.toFixed(2)}
`;

}

function gerarPDF(){
    window.print();
}

</script>

</body>
</html>
