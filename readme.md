# TV-Hub TradingView Boilerplate / Template Strategy


This repository contains a **boilerplate strategy** for TradingView PineScript developers to easily connect their trading strategies with **[TradingView Hub](https://www.tv-hub.org)**. The script provides a customizable input settings system that generates trade commands to be executed via automated alerts through TV-Hub. It is designed to serve as a starting template for developers to build and integrate their own strategies in order to execute their signals directly on major crypto exchanges.

Simply integrate the **TV-Hub input settings** section into your existing strategy. The script automatically generates **buy**, **sell**, and **close** trade commands, which can be triggered via alerts. These commands are sent through webhooks to **[TV-Hub](https://www.tv-hub.org)**, where they are executed on the exchange of your choice.

---

## Why Use This Script? 🛠️

This script is designed to save developers time by providing a **pre-built framework** for executing their PineScript trade signals seamlessly on major exchanges via **[TV-Hub](https://www.tv-hub.org)**. It simplifies the process of creating dynamic trade commands with custom risk management and order type settings. By using this template, developers can focus on building their trading logic while effortlessly connecting to an automated trading system.
This approach also makes it easy for developers to distribute and sell their strategies to a wide range of traders who are interested in using high-quality, automated trading solutions.

This script makes it incredibly easy to integrate the majority of TV-Hub's powerful features directly into any TradingView strategy. However, TV-Hub offers a wide array of additional functionalities that go beyond the scope of this template. We highly recommend exploring these features to fully leverage the platform’s capabilities. For instance, TV-Hub includes a **Copy-Trading Engine**, specialized order types for closing trades (e.g., close by limit or partial close), dedicated commands for managing stop-loss and take-profit orders, a **Telegram Trading Engine**, and much more. To maximize your experience, make sure to check out our comprehensive **[documentation](https://www.tv-hub.org/Home/Documentation)** and familiarize yourself with the **Trade Command Builder**.

---

## Features ✨

- **Automated Trade Execution** via the **[TV-Hub](https://www.tv-hub.org)** platform.
- **Customizable Input Settings** for easy configuration of trade parameters.
- **Dynamic Risk Management** including position sizing, leverage, and margin type.
- **Flexible Order Types** with options for market and limit orders, including limit order chasing.
- **Take-Profit and Stop-Loss Settings** with both percentage and absolute options.
- **Trailing Stop and Break Even** settings to further enhance trade management.
- **Dollar Cost Averaging (DCA)** for automated order distribution.
- **JSON Command Generation** for trade execution through webhooks.

---

## Input Parameters 🔧

The parameters are grouped based on their settings categories for easy configuration. Here's a breakdown of the input parameters available in the script:

### ⚙️ Basic Settings

- **`token`**: Your TV-Hub Access-Key. This key is used to authenticate with the TV-Hub platform. You must be a registered user on **[TV-Hub](https://www.tv-hub.org)**. You can find your access key under the profile settings.

- **`apiName`**: The name of the API key from the exchange where you want to trade (e.g., Binance, Bybit). You must add this API key to your TV-Hub account, and you should specify the exact name used when adding it to TV-Hub.

- **`exchangeName`**: Select the exchange for trading. Available options include Bybit, Binance, Bitmex, and OKX.

### 🏷️ Order Type Settings

- **`orderType`**: Select the order type (`Market` or `Limit`). Market orders are executed immediately, while limit orders are placed at the specified price.

- **`enableBestPrice`**: When using limit orders, this option ensures that TV-Hub places the order at the current best bid or ask price from the order book. If this is not enabled, the strategy will use the specified entry price (e.g., the close price). This setting is only applicable when `orderType` is set to **Limit**.
  
- **`enableOrderChasing`**: This activates order chasing, which continuously updates the existing limit order to the current best bid or ask price while the order remains unfilled. The system will keep adjusting the limit order to track the best price until the order is either filled or the chasing period ends. It is dependent on both `orderType` being set to **Limit** and `enableBestPrice` being enabled. Without `enableBestPrice` activated, order chasing cannot be applied. If the order is not filled within the chasing period, it will be executed as a market order.

- **`orderChasingInterval`**: This parameter sets the interval, in seconds, for updating limit orders during the order chasing process. The default is 20 seconds, and it can be adjusted from 1 to 300 seconds. This setting is only relevant if `enableOrderChasing` is active. The total number of updates remains constant, so shorter intervals will result in a shorter chasing period (e.g., a 1-second interval results in 3 minutes of chasing). Be cautious of using short intervals, as exchanges may impose rate limiting.

### 📊 Risk Management Settings

- **`unitsType`**: Choose how to calculate position size. Options include:
  - `Use Fixed Size (USD)`
  - `Use Absolute Value (BTC, ETH)`
  - `Use Percent (Available)`
  - `Use Percent (Wallet)`
  - `Risk`
- **`units`**: This setting defines the position size for your trades, but the way you input the value depends on the `unitsType` you select:

    - **For `Use Fixed Size`**: Enter the position size in the quote asset (e.g., USDT for a BTC/USDT pair).
    - **For `Use Absolute Value`**: Enter the size in the base asset (e.g., BTC, ETH).
    - **For percentage-based options** (`Use Percent (Available)` or `Use Percent (Wallet)`): Enter the size as a percentage of your available balance or wallet size.

	- **For `Risk`**: Enter the risk as a percantage value. This option is only applicable when a stop-loss is enabled. The position size is calculated based on the percentage of your balance that you're willing to risk on the trade, relative to the stop-loss. This helps to maintain a predefined risk level, automatically adjusting the position size based on the stop-loss distance.
- **`margin`**: Select the margin type for the trade (`CROSSED` or `ISOLATED`).
- **`Leverage`**: Set the leverage for your trades (1x to 125x).

### 🎯 Take Profit Settings

- **`takeProfitType`**: This setting determines how take-profit levels are calculated.
    - If **Percent** is selected, each take-profit level is defined as a percentage of the entry price (e.g., TP1 is 1% above entry, TP2 is 2% above, etc.).
    - If **Absolute** is selected, the strategy must calculate the exact price targets for each take-profit level in the code at runtime. You don't need to input percentage values, but you must ensure the strategy calculates the absolute price targets dynamically.
  
- **`enableTakeProfit`**: This option enables the take-profit functionality for the strategy.
    - If `takeProfitType` is set to **Percent**, you must provide the percentage distance for each take-profit step (via `takeProfitDistance`).
    - If `takeProfitType` is set to **Absolute**, the strategy calculates the price targets automatically based on other factors, and this input can be left empty.

- **`takeProfitDistance`**: When using the **Percent** take-profit type, this defines the percentage distance between the entry price and each take-profit step.
    - For example, if you set `takeProfitDistance` to 1%, TP1 will be 1% above the entry, TP2 will be 2% above, and so on.
    - This value is only relevant if `takeProfitType` is set to **Percent**.

- **`targetAmountInPercent`**: This controls whether the size of the trade is distributed across multiple take-profit levels.
    - If enabled, the trade will be split across the different take-profit levels. The size distribution for each level is defined in the `sizePerTarget` field.

- **`sizePerTarget`**: This setting determines the size distribution across each take-profit level as a percentage.
    - For example, if you enable two take-profit levels (TP1 and TP2) and set `sizePerTarget` to 50%, then 50% of the position will be closed at TP1 and the remaining 50% at TP2.
    - The total across all take-profit levels should not exceed 100%. If 3 take-profit levels are enabled, and each is set to 33%, the total is 99%.

- **`TP1`, `TP2`, `TP3`, `TP4`, `TP5`**: Toggle each take-profit level on or off.

### 🛡️ Stop-Loss and Trailing Stop Settings

- **`stopLossType`**: Defines whether the stop-loss is calculated as a percentage or an absolute value. 
    - If **Percent** is selected, the stop-loss is based on a percentage of the entry price.
    - If **Absolute** is selected, the strategy must calculate and provide an exact stop-loss price.

- **`SL`**: Enables the stop-loss function. 
    - If **Percent** is chosen, the stop-loss percentage must be specified in the `stopLossPercentValue` field.
    - If **Absolute** is selected, the strategy calculates the stop-loss price at runtime, and the percentage field can remain empty.

- **`stopLossPercentValue`**: The percentage value of the stop-loss. Only relevant when `stopLossType` is set to **Percent**. This value defines how far below (or above for shorts) the stop-loss is placed from the entry price.

- **`trailing`**: Enables the trailing stop function, which adjusts the stop-loss as the price moves in your favor. 
    - The trailing stop follows the price based on the percentage specified in `stopValue`.

- **`stopValue`**: The percentage threshold that dictates how much the price needs to move in your favor before the stop-loss is adjusted. 
    - For example, if set to 3%, the stop-loss will trail the price when it moves 3% in your favor. The trailing stop is based purely on price movement, independent of leverage.

- **`enableBreakEven`**: Activates the break-even feature, which automatically moves the stop-loss to the entry price once the price reaches a certain threshold. 
    - This feature is dependent on the exchange and may not be supported on all platforms. Check the TV-Hub documentation for compatibility.

### 📈 Dollar Cost Averaging (DCA) Settings

Here’s the optimized version for the DCA (Dollar Cost Averaging) settings:

- **`enableDca`**: Enables DCA (Dollar Cost Averaging) to create an **order mesh** that distributes orders across multiple price levels. DCA helps capture price wicks or allows gradual entry into a position. The system evenly distributes the total order size over multiple trades within a specified price range. This can be particularly useful in volatile markets or for long-term cost averaging. Refer to the documentation for more detailed use cases.

- **`dcaPercent`**: Sets the **percentage range** for distributing the DCA orders. This is the price range within which the system will spread the orders. For example, if set to 0.4%, the orders will be placed within a 0.4% range from the first entry price.

- **`dcaOrderCount`**: Specifies the **number of DCA orders** to be placed within the defined price range. These orders will be evenly distributed, ensuring a smooth entry across the set range. For instance, if you specify 3 orders, the total order size will be split across 3 trades within the DCA range.

### ⚙️ Advanced Settings

- **`closeCurrentPosition`**: Close the current position when a signal is generated for the opposite direction (useful for flipping positions).

- **`preventPyramiding`**: Prevents pyramiding by ignoring signals when a position in the same direction is already open.

- **`cancelPendingOrders`**: Cancels all pending orders (limit and stop-loss) before placing a new trade.

- **`hedge`**: Enable hedge mode for holding both long and short positions simultaneously (Binance-Futures only).

---

## How to Use 📖

1. **Clone or download** this repository, or simply copy the input settings section into your existing strategy.
2. **Create an account** on **[TV-Hub](https://www.tv-hub.org)**.
3. **Add your API details** (Access-Key, API Name, and Exchange) to connect with TV-Hub.
4. **Customize the settings**, such as risk management, take profit, stop loss, and order types, to suit your strategy.
5. **Create a TradingView alert** and use the following placeholder in the alert message window: `{{strategy.order.alert_message}}`
5. **Deploy your strategy** on TradingView and let TV-Hub execute the signals via webhook.
6. **Optional**: Explore the additional features of TV-Hub and review the **[documentation](https://www.tv-hub.org/Home/Documentation)** to get the most out of the platform.

---

## License ⚖️

This project is licensed under the **MIT License**. For more details, refer to the [license here](https://opensource.org/licenses/MIT).

---

## Summary 🚀

With this setup, you can easily **create custom trade strategies** that execute automatically on TV-Hub. Feel free to modify, extend, and adapt this script to fit your needs! We also welcome your feedback—whether through pull requests or by reaching out to us on our **[Discord](https://discord.gg/3xqyXRm)** server. Happy coding! 🎉

If you build great strategies using this template, we’d love to see them! You might even be featured on our **[Partner Page](https://www.tv-hub.org/Home/Partners)**, showcasing your product to our growing user base.