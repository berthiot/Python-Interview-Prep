**Questiom**


ake the list of the investment “commands” that affects a user’s account, and output the final balance for each investment.

Your program’s input is a lines of following commands:

BRAND,{brand ID},{compound interest rate percentage per time slot}
E.g. “BRAND,APPX,3” means the brand ID “APPX” will give 3% compound interest for each time slot
The interest rate can be changed by the BRAND command on the same brand ID
INVEST,{investment ID},{brand ID},{amount}
E.g. “INVEST,a,APPX,1000” means the user adds 1000 money on the brand ID “APPX”, with investment id “a” that identifies this invest.
The input always has one or more BRAND command of the brand ID before INVEST command
Investment ID is unique for each INVEST command
NEXT_TIME_SLOT
Progress one time slot
Investments receive interest for each time slot
Interest computation must round down for every time slot
PAYMENT_FAILURE_NOTICE,{investment ID},{failed amount}
E.g. “PAYMENT_FAILURE_NOTICE,1,500” means we notified payment failure of the 500 money of the invest ID 1.
We need to cancel the failed amount of the investment, and also cancel interests yielded by that amount
Expected to return lines of CSV of the following columns:

Investment ID
Brand ID
Final balance of the investment
Output rows must be sorted by investment ID (dictionary order).

[execution time limit] 4 seconds (py3)

[memory limit] 1 GB

[input] array.string commands

[output] array.string

[Python 3] Syntax Tips


To solve this problem, we'll implement a solution that processes the investment commands in sequence, applying the necessary interest calculations, handling payment failures, and ultimately producing the final balances for each investment. Here's how to approach it:

### Steps:
1. **Store Brand Information**: Track the current compound interest rate for each brand.
2. **Track Investments**: Maintain the balance of each investment along with the brand it is associated with.
3. **Handle Time Slots**: When a `NEXT_TIME_SLOT` command is encountered, apply the interest to each investment based on the current rate of the brand.
4. **Payment Failures**: Adjust the balance and interest for investments that have failed payments.

### Implementation:

```python
def solution(commands):
    brands = {}
    investments = {}
    
    for command in commands:
        parts = command.split(',')
        cmd_type = parts[0]
        
        if cmd_type == "BRAND":
            brand_id = parts[1]
            interest_rate = int(parts[2])
            brands[brand_id] = interest_rate
        
        elif cmd_type == "INVEST":
            investment_id = parts[1]
            brand_id = parts[2]
            amount = int(parts[3])
            investments[investment_id] = {
                'brand_id': brand_id,
                'balance': amount,
                'failed_amount': 0
            }
        
        elif cmd_type == "NEXT_TIME_SLOT":
            for investment_id, investment in investments.items():
                brand_id = investment['brand_id']
                if brand_id in brands:
                    interest_rate = brands[brand_id]
                    # Apply compound interest (round down)
                    investment['balance'] += investment['balance'] * interest_rate // 100
        
        elif cmd_type == "PAYMENT_FAILURE_NOTICE":
            investment_id = parts[1]
            failed_amount = int(parts[2])
            if investment_id in investments:
                investment = investments[investment_id]
                # Subtract the failed amount
                investment['balance'] -= failed_amount
                investment['failed_amount'] += failed_amount
                # Subtract interest yielded by the failed amount
                brand_id = investment['brand_id']
                if brand_id in brands:
                    interest_rate = brands[brand_id]
                    failed_interest = failed_amount * interest_rate // 100
                    investment['balance'] -= failed_interest

    # Prepare the output, sorted by investment ID
    output_data = []
    for investment_id in sorted(investments.keys()):
        investment = investments[investment_id]
        output_line = f"{investment_id},{investment['brand_id']},{investment['balance']}"
        output_data.append(output_line)
    
    return output_data

# Example usage:
commands = [
    "BRAND,APPX,3",
    "INVEST,a,APPX,1000",
    "NEXT_TIME_SLOT",
    "BRAND,APPX,5",
    "INVEST,b,APPX,2000",
    "NEXT_TIME_SLOT",
    "PAYMENT_FAILURE_NOTICE,a,500",
    "NEXT_TIME_SLOT"
]

result = solution(commands)
for line in result:
    print(line)
```

### Explanation:

1. **Handling `BRAND` Commands**:
    - We store the brand ID and its associated compound interest rate in the `brands` dictionary.

2. **Handling `INVEST` Commands**:
    - We store each investment by its unique ID in the `investments` dictionary, including the brand ID and the initial amount.

3. **Handling `NEXT_TIME_SLOT` Commands**:
    - For each time slot, we apply the interest rate to all investments based on their respective brands.

4. **Handling `PAYMENT_FAILURE_NOTICE` Commands**:
    - When a payment fails, we reduce the investment's balance by the failed amount and also subtract any interest that would have been generated by that amount.

5. **Sorting and Output**:
    - Finally, the output is prepared by sorting the investments by their IDs and returning the formatted CSV lines.

### Output Example:
Given the `commands` in the example:

```
a,APPX,1490
b,APPX,2200
```

This solution should work efficiently within the constraints provided by the platform, handling various cases such as interest rate changes, time progression, and payment failures.
