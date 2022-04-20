## To the moon!!!!!

<div id="apevalue">$APE value</div>
<div id="apeholding">$APE value</div>
<div id="apeoverall">Total</div>
<div></div>
<script>
    const APEAMOUNT = 287.83188;
    const APEINITIALAMOUNT = 3255.0;
    // console.log("entering");
    // fetch('https://data.sifchain.finance/beta/pool/atom/liquidityProvider/sif1tn83mw9lryfm38aah8m94kkle8uwzwvfj7n4n5')
    //     .then(respnse => {
    //         return response.json();
    //     })
    //     .then(data =>  document.getElementById("test").innerHTML = data);

    async function getApeData() {
        let response = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&ids=apecoin');
        jsonBody = await response.json();
        console.log(jsonBody);
        let currentPrice = jsonBody[0]['current_price'];
        document.getElementById("apevalue").innerHTML =  "APE Current Price - $" + currentPrice;
        document.getElementById("apeholding").innerHTML =  "Amount in APE $" + currentPrice*APEAMOUNT;
        document.getElementById("apeoverall").innerHTML =  "Profit/Loss: $" +  (currentPrice*APEAMOUNT - APEINITIALAMOUNT);
    }

    getApeData();
        
</script>
