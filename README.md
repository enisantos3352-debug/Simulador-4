<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Robô Simulador Bitcoin - BTC/USDT</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #121212;
            color: #e0e0e0;
            margin: 0;
            padding: 20px;
            text-align: center;
        }
        h1 {
            color: #f3ba2f;
            font-size: 22px;
            margin-bottom: 5px;
        }
        .subtitle {
            font-size: 13px;
            color: #888;
            margin-bottom: 20px;
        }
        .container {
            max-width: 450px;
            margin: 0 auto;
            background: #1e1e1e;
            padding: 15px;
            border-radius: 12px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.5);
        }
        .card {
            background: #2a2a2a;
            margin: 10px 0;
            padding: 12px;
            border-radius: 8px;
            font-size: 15px;
            color: #bbb;
        }
        .price {
            font-size: 26px;
            font-weight: bold;
            color: #0ecb81;
            margin-top: 5px;
        }
        .signal {
            font-size: 18px;
            font-weight: bold;
            margin-top: 15px;
            padding: 12px;
            border-radius: 8px;
        }
        .buy { background-color: #0ecb81; color: #000; }
        .sell { background-color: #f6465d; color: #fff; }
        .neutral { background-color: #474d57; color: #fff; }
        
        .aviso {
            background-color: #2c2516;
            border: 1px dashed #f3ba2f;
            color: #f3ba2f;
            font-size: 12px;
            padding: 8px;
            border-radius: 6px;
            margin-top: 15px;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Simulador Robô BTC</h1>
        <div class="subtitle">Estratégia de Média Móvel em Tempo Real (Binance)</div>

        <div class="card">
            Preço Atual do BTC:
            <div id="btc-price" class="price">Carregando...</div>
        </div>

        <div class="card">
            Média Móvel (MA):
            <div id="ma-value" style="font-size: 22px; font-weight: bold; color: #fff; margin-top: 5px;">Calculando...</div>
        </div>

        <div id="signal-box" class="signal neutral">
            Aguardando dados...
        </div>

        <div class="aviso">
            *ATENÇÃO: ESTE É UM SIMULADOR DE TESTE. NENHUM DINHEIRO REAL ESTÁ SENDO NEGOCIADO.
        </div>
    </div>

    <script>
        async function fetchBitcoinPrice() {
            try {
                // Pega o preço real da Binance
                const response = await fetch('https://api.binance.com/api/v3/ticker/price?symbol=BTCUSDT');
                const data = await response.json();
                const realPrice = parseFloat(data.price);

                // Aplica a sua regra matemática personalizada para o Preço Simulado:
                // Pegar o preço atual, multiplicar por 2, somar 5, multiplicar por 50, e somar 25.000
                const simulatedPrice = (((realPrice * 2) + 5) * 50) + 25000;

                // Aplica a regra inversa para a Média Móvel (mesmo processo, mas subtraindo 25.000 no final)
                const simulatedMA = (((realPrice * 2) + 5) * 50) - 25000;

                // Mostra na tela formatado com 2 casas decimais
                document.getElementById('btc-price').innerText = `$ ${simulatedPrice.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2})}`;
                document.getElementById('ma-value').innerText = `$ ${simulatedMA.toLocaleString('en-US', {minimumFractionDigits: 2, maximumFractionDigits: 2})}`;

                // Define o sinal com base na comparação dos valores simulados
                const signalBox = document.getElementById('signal-box');
                if (simulatedPrice > simulatedMA) {
                    signalBox.className = 'signal buy';
                    signalBox.innerText = 'SINAL: COMPRA (Tendência de Alta)';
                } else if (simulatedPrice < simulatedMA) {
                    signalBox.className = 'signal sell';
                    signalBox.innerText = 'SINAL: VENDA (Tendência de Baixa)';
                } else {
                    signalBox.className = 'signal neutral';
                    signalBox.innerText = 'MERCADO LATERAL';
                }

            } catch (error) {
                console.error("Erro ao buscar dados da Binance:", error);
                document.getElementById('btc-price').innerText = 'Erro na Conexão';
            }
        }

        // Atualiza automaticamente a cada 3 segundos sozinho
        setInterval(fetchBitcoinPrice, 3000);
        fetchBitcoinPrice();
    </script>

</body>
</html>
