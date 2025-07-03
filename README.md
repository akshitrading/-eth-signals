# -eth-signals
“ETH trading signals with live price &amp; chart"
<!DOCTYPE html>
<html>
<head>
  <title>ETH Signals</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 20px;
      background-color: #f3f3f3;
    }
    #price {
      font-size: 24px;
      margin: 10px 0;
    }
    #signal {
      font-size: 28px;
      font-weight: bold;
      color: green;
    }
    iframe {
      border: none;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h1>-ETH Signals-</h1>
  <div id="price">Loading price...</div>
  <div id="signal">Loading signal...</div>

  <!-- TradingView Chart -->
  <iframe src="https://s.tradingview.com/widgetembed/?frameElementId=tradingview_dfdc1&symbol=BINANCE:ETHUSDT&interval=1&hidesidetoolbar=1&symboledit=1&saveimage=1&toolbarbg=f1f3f6&studies=[]&theme=light&style=1&timezone=Etc%2FUTC&withdateranges=1&hideideas=1&hidelegend=1&hide_side_toolbar=1&allow_symbol_change=true&details=1&hotlist=1&calendar=1&news=1&width=100%25&height=500" width="100%" height="500"></iframe>

  <script>
    async function getPrice() {
      try {
        const res = await fetch('https://api.coingecko.com/api/v3/simple/price?ids=ethereum&vs_currencies=usd');
        const data = await res.json();
        const ethPrice = data.ethereum.usd;
        document.getElementById("price").innerText = `ETH Price: $${ethPrice}`;

        // Signal logic
        let signal = "Hold";
        if (ethPrice < 2800) signal = "Buy";
        else if (ethPrice > 3300) signal = "Sell";

        const signalDiv = document.getElementById("signal");
        signalDiv.innerText = signal;
        signalDiv.style.color = signal === "Buy" ? "green" : signal === "Sell" ? "red" : "orange";

      } catch (err) {
        document.getElementById("price").innerText = "Error fetching price";
        document.getElementById("signal").innerText = "-";
      }
    }

    getPrice();
    setInterval(getPrice, 10000); // update every 10 seconds
  </script>
</body>
</html>
