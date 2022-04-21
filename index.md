## To the moon!!!!!


<link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">

  
<div class="w3-container">
    <table id="myTable" class="w3-table-all">
        <tbody>
            <tr>
                <th>Coin</th>
                <th>Amount</th>
                <th>Initial Amount</th>
                <th>Profit/Loss</th>
            </tr>
        </tbody>    
    </table>
  </div>
  <p id="totalAmount"></p>
  <p id="profitLoss"></p>
  



<script>

    //NAMES
    const COSMOS = "cosmos";
    const ETHEREUM = "ethereum";
    const SIFCHAIN = "sifchain";
    const USDCOIN = "usd-coin";
    const APECOIN = "apecoin";
    const PARAGEN = "paragen";
    const KADENA = "kadena";
    let coins = [COSMOS,ETHEREUM,SIFCHAIN,USDCOIN,APECOIN,KADENA,PARAGEN];

    //APE
    const APE = {
        "value":0,
        "amount":287.83188,
        "initialamount": 3255.0,
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
    const LIQUID = 2130.0;
        
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
            printCoin(APE);
            printCoin(KDA);
            printCoin(RGEN);
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
        })
    }

    function printFarm(farm) {
        // document.getElementById(farm["div1"]).innerHTML =  farm["token"] +"/ROWAN: $" +  farm["value"];
        // document.getElementById(farm["div2"]).innerHTML =  "Profit/Loss: $" +  farm["profit"];
        var tbodyRef = document.getElementById('myTable').getElementsByTagName('tbody')[0];

        var newRow = tbodyRef.insertRow();  
        var newCell = newRow.insertCell();  
        var newText = document.createTextNode(farm['token']);
        newCell.appendChild(newText);   
        var newCell = newRow.insertCell();  
        var newText = document.createTextNode(farm["value"]);
        newCell.appendChild(newText);
        var newCell = newRow.insertCell();  
        var newText = document.createTextNode(farm["initialAmount"]);
        newCell.appendChild(newText);
        var newCell = newRow.insertCell();  
        var newText = document.createTextNode(farm["profit"]);
        newCell.appendChild(newText);
        updateTotal();
    }

    function updateTotal(){
        document.getElementById("totalAmount").innerHTML =  "Total: "+ (cosmosfarm["value"]+ethereumfarm["value"]+usdcfarm["value"]+RGEN["value"]+KDA["value"]+APE["value"]);
        document.getElementById("profitLoss").innerHTML =  "Profit/Loss: "+ (cosmosfarm["profit"]+ethereumfarm["profit"]+usdcfarm["profit"]+RGEN["profit"]+KDA["profit"]+APE["profit"]);
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
        var newText = document.createTextNode(coin["value"]);
        newCell.appendChild(newText);   
        var newCell = newRow.insertCell();  
        var newText = document.createTextNode(coin["initialamount"]);
        newCell.appendChild(newText);
        var newCell = newRow.insertCell();  
        var newText = document.createTextNode(coin["profit"]);
        newCell.appendChild(newText);

        updateTotal();
    }

    getCoinsData();


</script>
