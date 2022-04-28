## To the moon!!!!!


<style>
    .center {
        display: block;
        justify-content: center;
        margin-top:50px;
        margin-bottom: 50px;
        margin-left: auto;
        margin-right: auto;
        height: 350px;
        width: 350px;
    }

    .marginLeft {
        display: block;
        float: left;
        padding-right: 30px;
        padding-left: 20px;
    }
</style>

<link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">

<div>
    <img class="center" src="https://preview.redd.it/cso30i95q0w01.jpg?auto=webp&s=ad0795ca473dfb1de54b27bc722c3f6fc066dacc">
</div>
<div class="w3-container">
    <table id="myTable" class="w3-table-all">
        <tbody>
            <tr>
                <th>Coin</th>
                <th>Amount</th>
                <th>Initial Amount</th>
                <th>Profit/Loss</th>
                <th>% Change</th>
            </tr>
        </tbody>    
    </table>
  </div>
  <p class="marginLeft" id="totalAmount"></p>
  <p class="marginLeft" id="profitLoss"></p>
  <p class="marginLeft" id="percentageChange"></p>
  



<script>
    const COSMOS = "cosmos";
    const ETHEREUM = "ethereum";
    const SIFCHAIN = "sifchain";
    const USDCOIN = "usd-coin";
    const APECOIN = "apecoin";
    const PARAGEN = "paragen";
    const KADENA = "kadena";
    const NOMINEX = "nominex";
    const BUSD = "binance-usd";
    const INITIALQTT = 27046;
    let coins = [COSMOS,ETHEREUM,SIFCHAIN,USDCOIN,APECOIN,KADENA,PARAGEN,NOMINEX,BUSD];

    const ETH = {
        "value":0,
        "amount":0.2213,
        "initialamount": 656.0,
        "ticker": "ETH",
        "name": ETHEREUM,
        "profit":0 
    }


    //APE
    const APE = {
        "value":0,
        "amount":287.83188 - 150.0, //287.83188 - 150.0
        "initialamount": 1598.85, //3255.0 - 450.12
        "ticker": "APE",
        "name": APECOIN,
        "profit":0
    };
    //PARAGEN
    const RGEN = {
        "value":0,
        "amount":10005.0,
        "initialamount": 2773.0,
        "ticker":"RGEN",
        "name": PARAGEN,
        "profit":0
    };
    //KADENA
    const KDA = {
        "value":0,
        "amount":392.87,
        "initialamount": 2605.0,
        "ticker":"KDA",
        "name": KADENA,
        "profit":0
    };
    //LIQUID

    const liquid = {
        "value" : 1308.0 + 3000,
        "profit": 0,
        "initialAmount": 1308.0,
        "token": "BUSD",
        "profit": 3000,
        "ticker": "BUSD"
    }
    
    const creepz = {
        "total":0,
        "value" :0,
        "profit": 0,
        "initialAmount": 1.79,
        "token": "CREEPZ",
        "ticker": "CREEPZ"
    }

    const nomiswap = {
        "value" :2871.0,
        "profit": 300,
        "initialAmount": 2200,
        "token": "NOMI",
        "ticker": "NOMI"
    }
        
    let cosmosfarm = {
        "ticker" : COSMOS,
        "sifchainPool" :"atom",
        "div1": "atom",
        "div2": "atomoverall",
        "token":"ATOM/ROWAN",
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
        "token":"ETH/ROWAN",
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
        "token":"USDC/ROWAN",
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
            printCoin(APE);
            printCoin(ETH);
            printCoin(KDA);
            printCoin(RGEN);
            printFarm(liquid);
            getNomiData();
            updateOpenSea();
        })

        //fetch farm coins tokens
        getFarmData(cosmosfarm);
        getFarmData(ethereumfarm);
        getFarmData(usdcfarm);
        
    }

    function getFarmData(farm) {

        fetch('https://fathomless-plateau-83860.herokuapp.com/https://data.sifchain.finance/beta/pool/'+farm['sifchainPool']+'/liquidityProvider/sif1tn83mw9lryfm38aah8m94kkle8uwzwvfj7n4n5')
        .then(response=>response.json())
        .then((data) => {
            let token1 = data["externalAsset"]["balance"]/farm['decimals'];
            let token2 = data["nativeAsset"]["balance"]/1000000000000000000;
            farm["value"] = token1*mymap.get(farm["ticker"]) + token2*mymap.get(SIFCHAIN)
            farm["profit"] = farm["value"] - farm["initialAmount"];
            printFarm(farm);
        })
    }


    function getNomiData() {

        fetch('https://nomi-api.herokuapp.com/')
        .then(response=>response.json())
        .then((data) => {
            const nomiValue = data["NMX"]*mymap.get(NOMINEX);
            const busdValue = data["BUSD"]*mymap.get(BUSD);
            const nomiRewards = data["REWARDS"]*mymap.get(NOMINEX);
            console.log(data["NMX"]);
            console.log(nomiValue,busdValue,nomiRewards);
            nomiswap["value"] = nomiValue+busdValue + nomiRewards;
            nomiswap["profit"] = nomiswap["value"]  - nomiswap["initialAmount"];
            printFarm(nomiswap);
        })
    }

    function printFarm(farm) {

        var tbodyRef = document.getElementById('myTable').getElementsByTagName('tbody')[0];

        var newRow = tbodyRef.insertRow();  
        var newCell = newRow.insertCell();  
        var newText = document.createTextNode(farm['token']);
        newCell.appendChild(newText);   
        var newCell = newRow.insertCell();  
        var newText = document.createTextNode(farm["value"].toFixed(2));
        newCell.appendChild(newText);
        var newCell = newRow.insertCell();  
        var newText = document.createTextNode(farm["initialAmount"].toFixed(2));
        newCell.appendChild(newText);
        var newCell = newRow.insertCell();  
        var newText = document.createTextNode(farm["profit"].toFixed(2));
        newCell.appendChild(newText);
        let percentage = ((farm["value"]/farm["initialAmount"])-1)*100
        var newCell = newRow.insertCell();  
        var newText = document.createTextNode(percentage.toFixed(2));
        newCell.appendChild(newText);
        updateTotal();
    }

    function updateOpenSea() {
        fetch('https://fathomless-plateau-83860.herokuapp.com/https://api.opensea.io/collection/genesis-creepz')
        .then(response=>response.json())
        .then((data) => {
            creepz["value"] = data["collection"]["stats"]["floor_price"];
            creepz["total"] = data["collection"]["payment_tokens"][0]["usd_price"]*creepz["value"];
            creepz["profit"] = creepz["value"] -creepz["initialAmount"];
            printFarm(creepz);
        })
    }

    function updateTotal(){
        let totalAmount = (cosmosfarm["value"]+ethereumfarm["value"]+usdcfarm["value"]+RGEN["value"]+KDA["value"]+APE["value"]+liquid["value"]+nomiswap["value"]+ETH["value"]+creepz["total"]).toFixed(2);
        document.getElementById("totalAmount").innerHTML =  "Total: "+ totalAmount;
        document.getElementById("profitLoss").innerHTML =  "Profit/Loss: "+ (cosmosfarm["profit"]+ethereumfarm["profit"]+usdcfarm["profit"]+RGEN["profit"]+ETH["profit"]+KDA["profit"]+APE["profit"]+liquid["profit"]+nomiswap["profit"]+(creepz["profit"]*mymap.get(ETHEREUM))).toFixed(2);
        document.getElementById("percentageChange").innerHTML =  "Percentage change: "+ (((totalAmount/INITIALQTT)-1)*100).toFixed(2) + "%";
    }


    function printCoin(coin) {
        let currentPrice = mymap.get(coin["name"]);
        coin["profit"] = currentPrice*coin["amount"] - coin["initialamount"]
        coin["value"] = currentPrice*coin["amount"]

        var tbodyRef = document.getElementById('myTable').getElementsByTagName('tbody')[0];

        var newRow = tbodyRef.insertRow();  
        var newCell = newRow.insertCell();  
        var newText = document.createTextNode(coin['ticker']);
        newCell.appendChild(newText);   
        var newCell = newRow.insertCell();  
        var newText = document.createTextNode(coin["value"].toFixed(2));
        newCell.appendChild(newText);   
        var newCell = newRow.insertCell();  
        var newText = document.createTextNode(coin["initialamount"].toFixed(2));
        newCell.appendChild(newText);
        var newCell = newRow.insertCell();  
        var newText = document.createTextNode(coin["profit"].toFixed(2));
        newCell.appendChild(newText);
        var newCell = newRow.insertCell();  
        let percentage = ((coin["value"]/coin["initialamount"])-1)*100
        var newText = document.createTextNode(percentage.toFixed(2));
        newCell.appendChild(newText);

        updateTotal();
    }


    
    getCoinsData();


</script>
