## To the moon!!!!!

<div id="apevalue">$APE value</div>
<div id="apeholding">$APE value</div>
<div id="apeoverall">Total</div>
<div></div>
<div></div>
<div>FARMS</div>
<div></div>
<div id="atom">ATOM FARM</div>
<div id="atomoverall">ATOM FARM</div>

<script>
    const APEAMOUNT = 287.83188;
    const APEINITIALAMOUNT = 3255.0;
    const ROWANATOMINITIALAMOUNT = 3240;
    const COSMOS = "cosmos";
    const ETHEREUM = "ethereum";
    const SIFCHAIN = "sifchain";
    const UOSDCOIN = "usd-coin";
    const APECOIN = "apecoin";
    let coins = [COSMOS,ETHEREUM,SIFCHAIN,UOSDCOIN,APECOIN];
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
        fetch('https://cors-anywhere.herokuapp.com/https://data.sifchain.finance/beta/pool/atom/liquidityProvider/sif1tn83mw9lryfm38aah8m94kkle8uwzwvfj7n4n5')
        .then(response=>response.json())
        .then((data) => {
            console.log("the data");
            console.log(data);
            let atom = data["externalAsset"]["balance"]/1000000;
            let rowan = data["nativeAsset"]["balance"]/1000000000000000000;
            console.log(atom,rowan);
            printAtom(atom,rowan);
        })
    }

    function printAtom(atom,rowan) {
        let amount = atom*mymap.get(COSMOS) + rowan*mymap.get(SIFCHAIN)
        document.getElementById("atom").innerHTML =  "Atom/Rowan: $" +  amount;
        document.getElementById("atomoverall").innerHTML =  "Profit/Loss: $" +  (amount - ROWANATOMINITIALAMOUNT);
    }

    function printApe() {
        console.log(mymap);
        let currentPrice = mymap.get(APECOIN);
        console.log(currentPrice);
        document.getElementById("apevalue").innerHTML =  "APE Current Price - $" + currentPrice;
        document.getElementById("apeholding").innerHTML =  "Amount in APE $" + currentPrice*APEAMOUNT;
        document.getElementById("apeoverall").innerHTML =  "Profit/Loss: $" +  (currentPrice*APEAMOUNT - APEINITIALAMOUNT);
    }

    getCoinsData();

</script>
