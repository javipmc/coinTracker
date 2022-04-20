## To the moon!!!!!

<div id="apevalue">$APE value</div>
<div id="apeholding">$APE value</div>
<script>
    const APEAMOUNT = 287.83188;
    // console.log("entering");
    // fetch('https://data.sifchain.finance/beta/pool/atom/liquidityProvider/sif1tn83mw9lryfm38aah8m94kkle8uwzwvfj7n4n5')
    //     .then(respnse => {
    //         return response.json();
    //     })
    //     .then(data =>  document.getElementById("test").innerHTML = data);
    console.log("entering");

    async function getApeData() {
        let response = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&ids=apecoin');
        jsonBody = await response.json();
        console.log(jsonBody);
        document.getElementById("apevalue").innerHTML =  "APE Price - $" + jsonBody[0]['current_price'];
        document.getElementById("apeholding").innerHTML =  "Amount in APE $" + jsonBody[0]['current_price']*APEAMOUNT;
    }

    getApeData();
        
</script>
