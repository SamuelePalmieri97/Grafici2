<!DOCTYPE html>
<html>
<head>
  <title>Grafico Payoff</title>
  <!-- Includi Chart.js -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
  <!-- Elemento canvas per il grafico -->
  <canvas id="payoffChart"></canvas>
  
  <script>
    // Funzioni di payoff
    function longCall(strike, premium, prices) {
      return prices.map(price => ({ x: price, y: Math.max(price - strike, 0) - premium }));
    }

    function shortCall(strike, premium, prices) {
      return prices.map(price => ({ x: price, y: premium - Math.max(price - strike, 0) }));
    }

    function longPut(strike, premium, prices) {
      return prices.map(price => ({ x: price, y: Math.max(strike - price, 0) - premium }));
    }

    function shortPut(strike, premium, prices) {
      return prices.map(price => ({ x: price, y: premium - Math.max(strike - price, 0) }));
    }

    // Dati di esempio
    const strikePrice = 50;    // Prezzo di esercizio
    const premium = 5;         // Premio pagato/ricevuto
    const prices = Array.from({ length: 101 }, (_, i) => i); // Prezzi da 0 a 100

    const longCallData = longCall(strikePrice, premium, prices);
    const shortCallData = shortCall(strikePrice, premium, prices);
    const longPutData = longPut(strikePrice, premium, prices);
    const shortPutData = shortPut(strikePrice, premium, prices);

    // Configurazione del grafico
    const config = {
      type: 'line',
      data: {
        datasets: [
          {
            label: 'Call Long',
            data: longCallData,
            borderColor: 'blue',
            fill: false
          },
          {
            label: 'Call Short',
            data: shortCallData,
            borderColor: 'red',
            fill: false
          },
          {
            label: 'Put Long',
            data: longPutData,
            borderColor: 'green',
            fill: false
          },
          {
            label: 'Put Short',
            data: shortPutData,
            borderColor: 'purple',
            fill: false
          }
        ]
      },
      options: {
        scales: {
          x: {
            type: 'linear',
            position: 'bottom',
            title: { display: true, text: 'Prezzo del sottostante' }
          },
          y: {
            title: { display: true, text: 'Payoff' }
          }
        }
      }
    };

    // Crea il grafico nel canvas
    const ctx = document.getElementById('payoffChart').getContext('2d');
    new Chart(ctx, config);
  </script>
</body>
</html>
