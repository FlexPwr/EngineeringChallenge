# Engineering Challenge

* [Background](#Background)
* [Energy Trading in a Nutshell](#Energy-Trading-in-a-Nutshell)
* [The Challenge](#The-Challenge)
  * [Data Model](#Data-Model)
  * [Assignments](#Assignments)
* [Setup](#Setup)
* [Submission Instructions](#Submission-Instructions)
* [Notes](#Notes)


## Background
At flex power, software engineers:
- design, build and run reliable serverless systems.
- collaborate closely with traders to build internal tools and help them get the best outcomes on the markets.
- work with market data to generate insights and data-driven trading decisions.

This challenge is meant to give you a taste of the type of problems our 
engineering team has to solve on a daily basis. It helps you decide if you might have fun working 
with us. 
It is also an opportunity to demonstrate your technical skills and the ability to understand and work with our domain.

## Energy Trading in a Nutshell
Energy trading happens in an **exchange**, a market where traders working for energy producers 
(solar plants, nuclear power plants, ...) and consumers (B2C energy providers, big 
energy consuming industries like steel and trains...) submit orders to buy and sell energy.
One of the major exchanges in Europe is called [**EPEX**](https://en.wikipedia.org/wiki/European_Power_Exchange).

These orders consist of a quantity (in Megawatt) over a predefined period of
time, called **delivery period** (for example between 12:00, the **delivery start** and 13:00, 
the **delivery end**, on a given day) and for a given price per megawatt hour (referred to as mwh).

The orders are structured within orderbooks divided in bid (buy orders) and ask (sell orders).
![img.png](comtrader_snip.png)


If the prices of two orders with opposite sides match, i.e the buy price is higher than the sell price, 
then, a trade is generated. 
For example if the orderbook contains an order to sell 10 mw for 10 euros/mwh and another trader 
submits an order to buy 5 mw for 11 euros/mwh, the orders are matched by the exchange and a trade is 
generated for 5mw at 11 euros/mwh.

Those trades are saved by flex power and used to compute various indicators on the 
performance of our trading strategies. 

The challenge bases on a collection of such trades and tries to implement a couple of functionalities
flex power has. 

## The Challenge

### Short description:
We want to setup a minimal reporting tool for different trading strategies.

### Data Model
The challenge models a **trade** as follows:
- Trade:
  - id: integer
  - price: float
  - quantity: integer
  - side: string, buy or sell
  - strategy_id: string

### Assignments
#### Task 1: 
Write a function that computes the total buy volume for flex power, another that computes the total sell volume.
```python

def compute_total_buy_volume(*args, **kwargs) -> float:
    pass

def compute_total_sell_volume(*args, **kwargs) -> float:
    pass
```

#### Task 2: 
Write a function that computes the PnL (profit and loss) of each strategy in euros. It's defined as the sum of the incomes 
realized with each trade.

If we sell energy, our income is `quantity * price` since we got money for our electricity. If we buy energy, our income is `-quantity * price`.
```python
def compute_total_sell_volume(strategy_id: str, *args, **kwargs) -> float:
    pass
```
This function should return 0 it there are no trades that correspond to the strategy id.

#### Task3: 
Expose the function defined in the second task as an entrypoint of a web application. 

Here is the corresponding API definition.
```yaml
swagger: "2.0"
info:
  title: Energy Trading API
  version: 1.0.0
host: api.example.com
basePath: /v1
schemes:
  - https
paths:
  /pnl/{strategy_id}:
    get:
      summary: Returns the pnl of the corresponding strategy.
      parameters:
        - in: path
          name: user_id
          required: true
          type: string
          description: string identifier of a strategy.
      produces:
        - application/json
      responses:
        200:
          description: A PnL data object.
          schema:
            type: object
            properties:
              strategy:
                type: string
                example: my_strategy
              value:
                type: float
                example: 100.0
              unit:
                type: string
                example: euro
              capture_time:
                type: string
                example: "2023-01-16T08:15:46Z"
```

#### Task 4
package your application and provide us with clear instructions on how to run it. 

PS: It is preferable that you use docker.

## Setup
The repository contains a sqlite file `trades.sqlite`, containing a database named `trades`.

The database contains a table called `epex_2022_12_20_12-13`, with all trades made by flex power traders 
 on the EPEX exchange for the delivery period 12:00 to 13:00 on 2022-12-20.

The schema is the following
```sqlite
id TEXT PRIMARY KEY,
quantity INTEGER NOT NULL,
price REAL NOT NULL,
side TEXT NOT NULL CHECK (side IN ('buy', 'sell')),
strategy TEXT NOT NULL
```

## Submission Instructions
Create a Github repository containing your solution and provide us with access so that we can review your 
code.

Feel free to add notes about technology choices and design decisions, as well as anything that we should
keep in mind when reviewing your code.

Ideally, the deliverable should be in python 3.*.

## Notes
* Spend as much time as you want on the challenge to produce something you are proud of. The intended time is a couple 
of hours.
* The solution is important but so is everything else around it: unit tests, commit size and description...
* If you can think of any other cool features that you could implement, go for it! Just be sure to describe them in the readme.
* If you already have similar personal projects that you would like to submit instead of this one, it's also possible.
Just make sure that you include an exhaustive documentation on what it's about and how we can start it.
