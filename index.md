## To the moon!!!!!

<!DOCTYPE html>

<div id="apevalue">$APE value</div>
<div id="apeholding">$APE value</div>
<div id="apeoverall">Total</div>
<div></div>
<div></div>
<div>FARMS</div>
<div></div>
<div id="atom">ATOM FARM</div>
<div id="atomoverall">ATOM FARM</div>
<div id="ethereum">ETHEREUM FARM</div>
<div id="ethereumoverall">ETHEREUM FARM</div>
<div id="usdc">USDC FARM</div>
<div id="usdcoverall">USDC FARM</div>
<div id="totalFarms">Total</div>

<script>
    const APEAMOUNT = 287.83188;
    const APEINITIALAMOUNT = 3255.0;
    const COSMOS = "cosmos";
    const ETHEREUM = "ethereum";
    const SIFCHAIN = "sifchain";
    const USDCOIN = "usd-coin";
    const APECOIN = "apecoin";
    let coins = [COSMOS,ETHEREUM,SIFCHAIN,USDCOIN,APECOIN];
    
    let cosmosfarm = {
        "ticker" : COSMOS,
        "sifchainPool" :"atom",
        "div1": "atom",
        "div2": "atomoverall",
        "token":"ATOM",
        "value":0,
        "profit":0,
        "initialAmount":3240,
        "decimals":1000000
    }

    let ethereumfarm = {
        "ticker" : ETHEREUM,
        "sifchainPool" :"eth",
        "div1": "ethereum",
        "div2": "ethereumoverall",
        "token":"ETH",
        "value":0,
        "profit":0,
        "initialAmount":3000,
        "decimals":1000000000000000000
    }

    let usdcfarm = {
        "ticker" : USDCOIN,
        "sifchainPool" :"usdc",
        "div1": "usdc",
        "div2": "usdcoverall",
        "token":"USDC",
        "value":0,
        "profit":0,
        "initialAmount":2400,
        "decimals":1000000
    }

    let mymap;


    var queryString = coins.join(',')


    function getCoinsData() {

        //fetch all coins prices
        fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&ids='+queryString)
        .then((response) => response.json())
        .then((data) => {
            mymap = new Map(data.map(object => [object["id"],object["current_price"]]));
            printApe();
        })

        //fetch farm coins tokens
        getFarmData(cosmosfarm);
        getFarmData(ethereumfarm);
        getFarmData(usdcfarm);
    }

    function getFarmData(farm) {

        fetch('https://cors-anywhere.herokuapp.com/https://data.sifchain.finance/beta/pool/'+farm['sifchainPool']+'/liquidityProvider/sif1tn83mw9lryfm38aah8m94kkle8uwzwvfj7n4n5')
        .then(response=>response.json())
        .then((data) => {
            let token1 = data["externalAsset"]["balance"]/farm['decimals'];
            let token2 = data["nativeAsset"]["balance"]/1000000000000000000;
            console.log(mymap);
            farm["value"] = token1*mymap.get(farm["ticker"]) + token2*mymap.get(SIFCHAIN)
            farm["profit"] = farm["value"] - farm["initialAmount"];
            printFarm(farm);
            updateTotalFarms();
        })
    }

    function printFarm(farm) {
        document.getElementById(farm["div1"]).innerHTML =  farm["token"] +"/ROWAN: $" +  farm["value"];
        document.getElementById(farm["div2"]).innerHTML =  "Profit/Loss: $" +  farm["profit"];
    }

    function updateTotalFarms(){
        document.getElementById("totalFarms").innerHTML =  "TOTAL FARMS: "+ (cosmosfarm["profit"]+ethereumfarm["profit"]+usdcfarm["profit"]);
    }

    function printApe() {
        let currentPrice = mymap.get(APECOIN);
        console.log(currentPrice);
        document.getElementById("apevalue").innerHTML =  "APE Current Price - $" + currentPrice;
        document.getElementById("apeholding").innerHTML =  "Amount in APE $" + currentPrice*APEAMOUNT;
        document.getElementById("apeoverall").innerHTML =  "Profit/Loss: $" +  (currentPrice*APEAMOUNT - APEINITIALAMOUNT);
    }

    getCoinsData();

</script>
