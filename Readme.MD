Creating an automated script for daily activities on a Mac can be done using a combination of shell scripts, Automator, and cron jobs (or launchd for more advanced scheduling). Here's an example script to automate checking wallet balances, reviewing transactions, and tracking market prices. This example assumes you have the necessary APIs or command-line tools to interact with your crypto wallets and market data.

### Step 1: Install Required Tools

Make sure you have the necessary tools installed. You might need `curl` for API requests, `jq` for JSON parsing, and a tool like `crypto` for checking wallet balances.

```bash
brew install curl jq
```

### Step 2: Create the Shell Script

Create a shell script that performs your daily activities.

```bash
#!/bin/bash

# Define your API endpoints and keys
WALLET_ADDRESS="your_wallet_address"
API_KEY="your_api_key"
MARKET_API="your_market_api_endpoint"

# Function to check wallet balance
check_wallet_balance() {
  balance=$(curl -s "https://api.yourwalletservice.com/balance/$WALLET_ADDRESS?apikey=$API_KEY" | jq '.balance')
  echo "Wallet Balance: $balance"
}

# Function to review recent transactions
review_transactions() {
  transactions=$(curl -s "https://api.yourwalletservice.com/transactions/$WALLET_ADDRESS?apikey=$API_KEY" | jq '.transactions')
  echo "Recent Transactions: $transactions"
}

# Function to track market prices
track_market_prices() {
  prices=$(curl -s "$MARKET_API?apikey=$API_KEY" | jq '.prices')
  echo "Market Prices: $prices"
}

# Main function to run daily tasks
run_daily_tasks() {
  echo "Starting daily tasks..."
  check_wallet_balance
  review_transactions
  track_market_prices
  echo "Daily tasks completed."
}

# Run the main function
run_daily_tasks
```

Save this script as `daily_tasks.sh` and make it executable:

```bash
chmod +x daily_tasks.sh
```

### Step 3: Create a Cron Job

Set up a cron job to run this script daily. Open your cron tab with:

```bash
crontab -e
```

Add the following line to schedule the script to run every day at a specific time (e.g., 8 AM):

```bash
0 8 * * * /path/to/your/daily_tasks.sh
```

Replace `/path/to/your/daily_tasks.sh` with the actual path to your script.

### Step 4: Using Automator for GUI Notifications (Optional)

If you want to get macOS notifications, you can use Automator to create an application that runs the script and then sends a notification.

1. Open Automator and create a new "Application".
2. Add a "Run Shell Script" action.
3. Paste your script into the "Run Shell Script" action.
4. Add a "Show Notification" action to display a message when the script completes.
5. Save the Automator application.

Now, you can run this Automator application as part of your cron job or manually to get GUI notifications.

### Example Automator Notification Script

Replace the `run_daily_tasks` function in your shell script with the following to include macOS notifications:

```bash
run_daily_tasks() {
  echo "Starting daily tasks..."
  check_wallet_balance
  review_transactions
  track_market_prices
  osascript -e 'display notification "Daily tasks completed" with title "Daily Activities"'
  echo "Daily tasks completed."
}
```

This script will now display a macOS notification when the daily tasks are completed.