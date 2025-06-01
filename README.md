# SecurityOfSmartContracts
To Secure Smart Contracts from potential threats and risks through industry proven audit/review frameworks. (referred from blogs and posts by Cyfrin and Solidit(@hansfriese) (i.e. https://solodit.cyfrin.io/). 

**Contents:**

(1) [Denial-of-Service Attacks - Part01](https://github.com/MUdayVarma/SecurityOfSmartContracts?tab=readme-ov-file#1-denial-of-service-attacks)

(2) [Denial-of-Service Attacks - Part02](https://github.com/MUdayVarma/SecurityOfSmartContracts?tab=readme-ov-file#2-denial-of-service-attacks---part02)

(3) Donation Attacks
 

------------------------------------------------- 

## (1) Denial-of-Service Attacks - Part01

‍DoS attacks are about exploiting vulnerabilities in your code, turning your smart contract unusable, either for specific users or for everyone. 
They're like digital roadblocks, preventing legitimate folks from accessing your protocol's well-intentioned services! 

To illustrate, think of it like this: You've poured your heart and soul into building a beautiful coffee shop. It's the best coffee shop in the world. 
But then someone comes and keeps ordering thousands of empty cups of coffee, clogging up the entire system and preventing real, paying customers from getting their caffeine fix. That's a DoS attack in a nutshell! 

This are three checklist items:
- Is the withdrawal pattern followed to prevent denial of service?
- Is there a minimum transaction amount enforced?
- How does the protocol handle blacklisting functionality tokens?

### High-level overview
DoS attacks are usually about leveraging design flaws in your smart contract logic. It's not brute force. They are using subtle manipulation to beat you.Here's a breakdown of the general idea:

**Logic exploitation**: This is all about finding clever (or, more accurately, malicious) ways to trigger state changes that lead to reverted transactions or infinite loops, effectively freezing the contract. 
This happens more often than you might think.

**Resource exhaustion**: Think of this like the tragedy of the commons but with gas fees. It occurs when the attacker floods the contract with requests, consuming excessive gas and making it prohibitively expensive for legitimate users.
Remember, every action on the blockchain costs gas, and a flood of transactions can catapult those costs.

### Summary
Denial-of-Service (DoS) attack: An attack that attempts to disrupt or make a smart contract or its functions unavailable to legitimate users. This can be achieved through consuming excessive gas, causing contract reverts, or exploiting vulnerabilities to block critical operations.

‍**Withdrawal pattern**: A secure smart contract design pattern that emphasizes a "pull" model for withdrawals. Instead of a contract pushing tokens to users, users initiate the withdrawal process, mitigating potential issues like DoS vulnerabilities caused by failing recipient transfers.

**‍Dust transactions**: These are extremely small-value transactions, often used maliciously to clog a network or smart contract, making legitimate transactions more expensive or hindering their processing.

**‍Token blacklisting**: A feature implemented by some token contracts (e.g., certain ERC-20 implementations) that allows specific addresses to be blocked from transferring tokens. This effectively freezes their funds or prevents them from interacting with the token.


Read (for more detailed explaination): https://www.cyfrin.io/blog/solodit-checklist-explained-denial-of-service-attacks-1 


------------------------------------------------- 

## (2) Denial-of-Service Attacks - Part02

Learn how to prevent denial-of-service (DoS) attacks in smart contracts by securing queues, handling low-decimal tokens, and managing external calls safely. We'll delve into queue processing vulnerabilities, the challenges presented by low-decimal tokens, and the importance of handling external contract calls safely. 

### A brief overview

**Blocking Queue Processing**: This problem occurs when an attacker manipulates a queue to halt or disrupt its operation, leading to a DoS. We'll examine how attackers can compromise processing queues.
 
_Remediation:_
      - Restrict modifying withdrawalRequested status to the admin.
      - Ensure validation checks to avoid zero-value transactions.
      - Implement a fallback function to handle unexpected errors.


**Low Decimal Tokens**: The issue arises when dealing with tokens with a low number of decimals. Calculations, particularly divisions, can be truncated down to zero, leading to unexpected and detrimental behavior. Such low-precision tokens can lead to integer division issues that disrupt key functions.

_Remediation:_ Ensure the contract handles low decimal tokens correctly by scaling math formulas to mitigate integer rounding during calculations.


**Unsafe External Calls**: We'll investigate how reliance on external contracts without proper error handling can create vulnerabilities. The problem occurs when the failure of an external call isn't managed correctly, potentially causing the entire contract to revert.

_Remediation:_ Wrap external contract calls in try/catch blocks to handle reverted errors and implement a fallback or cached value.



Read (for more detailed explaination): https://www.cyfrin.io/blog/solodit-checklist-explained-denial-of-service-attacks-2

------------------------------------------------- 

## (3) Donation Attacks

**Description:** Attackers can manipulate the accounting by donating tokens.

_Remediation_: Implement internal accounting instead of relying on `balanceOf` natively.

### A brief overview

A Donation Attack exploits vulnerabilities in how a contract manages token balances. It often stems from an incorrect state due to an assumption of external factors. The core of the vulnerability lies in the attacker's ability to manipulate the contract's state. Imagine an attacker cleverly "donating" tokens directly to a contract instead of using the protocol's interface. This seemingly altruistic act can severely disrupt the contract's logic and lead to unfair distributions of assets, potentially harming legitimate users. The attacker can manipulate the contract's state if the protocol does not account for such direct transfers and solely relies on the token balance for accounting.

This item aims to prevent the Donation Attack vulnerability by ensuring the protocol does not rely on external functions like balanceOf or balance for accounting. This checklist item directly relates to internal accounting - a method of accurately tracking asset ownership and balances within a smart contract. Internal accounting utilizes dedicated state variables to store and manage balances, rather than relying on external functions like balanceOf.

The vulnerability in external accounting is that anyone can send tokens directly to a contract, regardless of intended logic. If your contract uses token.balanceOf(address(this)) to calculate shares, withdrawals, or any critical value, an attacker can donate tokens, irrevocably compromising the system and potentially altering the intended outcome.


------------------------------------------------- 
