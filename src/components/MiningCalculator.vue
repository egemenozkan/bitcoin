<template>
    <div class="hello">
        <h1>Bitcoin Mining ROI Calculator</h1>
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="form-group">
                      <label for="tb-todayBtcUsd">BTC Today Value</label>
                      <input id="tb-todayBtcUsd" type="number" class="form-control" v-model="todayBtcUsd" placeholder="BTC/USD" />
                      <small id="btcHelp" class="form-text text-muted">Please, change BTC/USD rate how much will be worth end of the contract.</small>
                    </div>

                </div>
                <div class="col-lg-3 col-md-3 col-12" v-bind:key="index" v-for="(option,index) in miningOptions">
                  <div class="card">
                    <div class="card-header">
                        <h2 v-text="option.name"></h2>
                    </div>
                    <div class="card-body">
                        <div>{{ option.power.text }}</div>
                        <div>{{ option.cost.amount }}{{ option.cost.currency }}</div>
                        <div>{{ option.contract.day }} days</div>
                        <div>{{ option.contract.startAfterPurchase }} days</div>
                        <div>
                          <h3>End Of Contract</h3>
                          <div><label>Mining</label>{{ option.collection.BTC }} Bitcoin </div>
                          <div><label>Net</label>{{ option.collection.netBTC }} Bitcoin </div>
                          <div><label>Net</label>{{ option.collection.netUSD }} USD </div>
                          <div><label>ROI</label>{{ option.rateOfInvestment }} %</div>
                        </div>
                    </div>
                    </div>
                </div>
            </div>
        </div>
        <table>
            <thead>
                <tr>
                    <th></th>
                    <th></th>
                    <th></th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td></td>
                    <td></td>
                    <td></td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
/*jshint -W109 */

import "bootstrap/dist/css/bootstrap.css";
import "bootstrap-vue/dist/bootstrap-vue.css";

var todayBtcUsd = 8860;

function dayCountBeetween(date0, date1) {
    if (date0 == "today") {
        date0 = new Date();
    } else {
        date0 = new Date(date0);
    }
    if (date1 == "today") {
        date1 = new Date();
    } else {
        date1 = new Date(date1);
    }
    return Number(((date1 - date0) / (1000 * 60 * 60 * 24)).toFixed(0));
}

var halvingPeriods = { '12.5' : {
        date: "2020-05-14",
        reward: 12.5,
        exact: true,
        halving: 1
    },
    '6.25' : {
        date: "2024-05-14",
        reward: 6.25,
        exact: false,
        halving: 0.5
    }
};

var miningPower = {
    TH: {
        amount: 1,
        unit: "Th/s",
        dailyReward: {
            amount: 0.00001837,
            currency: "BTC"
        },
        dailyCost: {
            electric: {
                amount: 0.342,
                currency: "USD"
            },
            pool: {
                amount: 0.01,
                currency: "BTC/%"
            }
        }
    }
};

var miningOptions = [{
        name: "Standard",
        power: {
            text: "5 TH/s",
            amount: 5
        },
        cost: {
            amount: 500,
            currency: "USD"
        },
        contract: {
            day: 1400,
            startAfterPurchase: 10
        },
        collection: {
            BTC: 0,
            netBTC: 0,
            netUSD: 0
        },
        rateOfInvestment: 0
    },
    {
        name: "Bronze",
        power: {
            text: "10 TH/s",
            amount: 10
        },
        cost: {
            amount: 1000,
            currency: "USD"
        },
        contract: {
            day: 1400,
            startAfterPurchase: 10
        },
        collection: {
            BTC: 0,
            netBTC: 0,
            netUSD: 0
                },
        rateOfInvestment: 0
    },
    {
        name: "Silver",
        power: {
            text: "15 TH/s",
            amount: 15
        },
        cost: {
            amount: 1500,
            currency: "USD"
        },
        contract: {
            day: 1400,
            startAfterPurchase: 10
        },
        collection: {
            BTC: 0,
            netBTC: 0,
            netUSD: 0
               },
        rateOfInvestment: 0
    },
    {
        name: "Gold",
        power: {
            text: "35 TH/s",
            amount: 35
        },
        cost: {
            amount: 3500,
            currency: "USD"
        },
        contract: {
            day: 1400,
            startAfterPurchase: 10
        },
        collection: {
            BTC: 0,
            netBTC: 0,
            netUSD: 0
               },
        rateOfInvestment: 0
    }
];

export default {
    name: "MiningCalculator",
    props: {},
    data: function() {
        return {
            investment: {
                amount: 500,
                currency: "USD"
            },
            todayBtcUsd : todayBtcUsd
        };
    },
    methods: {},
    computed: {
        miningOptions: function() {
            var self = this;

            var remainingDayToFirstHalving = dayCountBeetween(
                "today",
                halvingPeriods['12.5'].date
            );
            /** Calculation until first (end of 12.5 BTC Reward for block) halving period */
            var fhpReward =
                miningPower['TH'].dailyReward.amount * (remainingDayToFirstHalving  > 0 ? remainingDayToFirstHalving : 0);
            console.log("first halving reward: ",fhpReward,":->", remainingDayToFirstHalving);
            var remainingDayOfContractExpire = 1400 - (remainingDayToFirstHalving  > 0 ? remainingDayToFirstHalving : 0);

            /** Calculation until second (end of 6.25 BTC Reward for block) halving period */
            var remainingDayToSecondHalving = dayCountBeetween(
                halvingPeriods['12.5'].date,
                halvingPeriods['6.25'].date
            );

            if (remainingDayOfContractExpire > remainingDayToSecondHalving) {
                console.error("Update Source Code");
                return;
            }

            var shpReward =
                miningPower['TH'].dailyReward.amount *
                halvingPeriods['6.25'].halving *
                remainingDayOfContractExpire;

            console.log("second halving reward: ",shpReward,":->", remainingDayOfContractExpire);

            for (let index = 0; index < miningOptions.length; index++) {
                var miningOption = miningOptions[index];
                var poolCommisionRatio =  (1 - miningPower['TH'].dailyCost.pool.amount) ;
                var electricCostAsBTC = miningPower['TH'].dailyCost.electric.amount / self.todayBtcUsd;
                console.log("poolCommisionRatio:: ", poolCommisionRatio, "electricCostAsBTC:: ",electricCostAsBTC);
                miningOption.collection.BTC = (fhpReward *  miningOption.power.amount) +  (shpReward * miningOption.power.amount);
                miningOption.collection.BTC = Number(miningOption.collection.BTC.toFixed(8))
                miningOption.collection.netBTC = (((fhpReward * poolCommisionRatio) - electricCostAsBTC) * miningOption.power.amount) + (((shpReward * poolCommisionRatio) - electricCostAsBTC) * miningOption.power.amount);
                miningOption.collection.netBTC = Number(miningOption.collection.netBTC.toFixed(8))
                miningOption.collection.netUSD =   miningOption.collection.netBTC * self.todayBtcUsd;
                miningOption.collection.netUSD = miningOption.collection.netUSD.toFixed(2);

                /*https://www.quora.com/Whats-the-difference-between-ROR-Rate-of-Return-and-ROI-Return-on-Investment */
                miningOption.rateOfInvestment = (((miningOption.collection.netUSD - miningOption.cost.amount)/miningOption.cost.amount) * 100).toFixed(2);
                console.log(index, JSON.stringify(miningOption.collection));
            }

            return miningOptions;
        }
    }
};
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped lang='less'>
h3 {
  font-size: 18px;
margin: 15px;
border-bottom: 1px solid;
padding-bottom: 5px;

}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    display: inline-block;
    margin: 0 10px;
}

a {
    color: #42b983;
}

label {
  padding: 0 5px;
}
</style>
