# SecurityOfSmartContracts
To Secure Smart Contracts from potential threats and risks through industry proven audit/review frameworks. (referred from blogs and posts by Cyfrin and Solidit(@hansfriese) (i.e. https://solodit.cyfrin.io/). 

**Contents:**

(1) [Denial-of-Service Attacks](https://github.com/MUdayVarma/SecurityOfSmartContracts/new/main?readme=1#1-denial-of-service-attacks)
 

------------------------------------------------- 

## (1) Denial-of-Service Attacks

‍DoS attacks are about exploiting vulnerabilities in your code, turning your smart contract unusable, either for specific users or for everyone. 
They're like digital roadblocks, preventing legitimate folks from accessing your protocol's well-intentioned services! 

To illustrate, think of it like this: You've poured your heart and soul into building a beautiful coffee shop. It's the best coffee shop in the world. 
But then someone comes and keeps ordering thousands of empty cups of coffee, clogging up the entire system and preventing real, paying customers from getting their caffeine fix. That's a DoS attack in a nutshell! 

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


