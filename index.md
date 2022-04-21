## To the moon!!!!!

<div id="apevalue">$APE value</div>
<div id="apeholding">$APE value</div>
<div id="apeoverall">Total</div>
<div></div>
<div></div>
<div>FARMS</div>
<div></div>
<div id="atom">ATOM FARM</div>

<script>
    const APEAMOUNT = 287.83188;
    const APEINITIALAMOUNT = 3255.0;
    const COSMOS = "cosmos";
    const ETHEREUM = "ethereum";
    const SIFCHAIN = "sifchain";
    const UOSDCOIN = "usd-coin";
    const APECOIN = "apecoin";
    let coins = [COSMOS,ETHEREUM,SIFCHAIN,UOSDCOIN,APECOIN];
    let mymap;
    // console.log("entering");
    // fetch('https://data.sifchain.finance/beta/pool/atom/liquidityProvider/sif1tn83mw9lryfm38aah8m94kkle8uwzwvfj7n4n5')
    //     .then(respnse => {
    //         return response.json();
    //     })
    //     .then(data =>  document.getElementById("test").innerHTML = data);

    var queryString = coins.join(',')

    // async function getCoinsData() {
    //     let response = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&ids='+queryString);
    //     jsonBody = await response.json();
    //     mymap = new Map(jsonBody.map(object => [object["id"],object["current_price"]]));
    //     console.log(mymap);
    // }

    function getCoinsData() {

        //fetch all coins prices
        fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&ids='+queryString)
        .then((response) => response.json())
        .then((data) => {
            mymap = new Map(data.map(object => [object["id"],object["current_price"]]));
            printApe();
        })

        //fetch farm coins tokens
        fetch('https://data.sifchain.finance/beta/pool/atom/liquidityProvider/sif1tn83mw9lryfm38aah8m94kkle8uwzwvfj7n4n5',{
            method: "GET", 
            mode: 'cors',
            headers: {
                'Content-Type': 'application/json',
            }
        })
        .then((response) => response.json())
        .then((data) => {
            console.log(data);
            let atom = data[0]["externalAsset"]["balance"]/1000000;
            let rowan = data[0]["nativeAsset"]["balance"]/1000000000000000000;
            console.log(atom,rowan);
            printAtom();
        })
    }

    function printAtom(atom,rowan) {
        document.getElementById("atom").innerHTML =  "Atom/Rowan: $" +  (atom*mymap.get(COSMOS) + rowan*mymap.get(SIFCHAIN));

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
