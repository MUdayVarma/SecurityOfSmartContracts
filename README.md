# SecurityOfSmartContracts
To Secure Smart Contracts from potential threats and risks through industry proven audit/review frameworks. (referred from blogs and posts by Cyfrin and Solidit(@hansfriese) (i.e. https://solodit.cyfrin.io/). 

**Contents:**

(1) [Denial-of-Service Attacks - Part01](https://github.com/MUdayVarma/SecurityOfSmartContracts?tab=readme-ov-file#1-denial-of-service-attacks)

(2) [Denial-of-Service Attacks - Part02](https://github.com/MUdayVarma/SecurityOfSmartContracts?tab=readme-ov-file#2-denial-of-service-attacks---part02)

(3) [Donation Attacks](https://github.com/MUdayVarma/SecurityOfSmartContracts/blob/main/README.md#3-donation-attacks) 

(4) [Front-Running Attacks](https://github.com/MUdayVarma/SecurityOfSmartContracts?tab=readme-ov-file#4-front-running-attacks)  

(5) [Griefing Attacks](https://github.com/MUdayVarma/SecurityOfSmartContracts?tab=readme-ov-file#5-griefing-attacks) 

(6) [Miner Attacks]()
 

------------------------------------------------- 

## (1) Denial-of-Service Attacks - Part01

‍DoS attacks are about exploiting vulnerabilities in your code, turning your smart contract unusable, either for specific users or for everyone. 
They're like digital roadblocks, preventing legitimate folks from accessing your protocol's well-intentioned services! 

To illustrate, think of it like this: You've poured your heart and soul into building a beautiful coffee shop. It's the best coffee shop in the world. 
But then someone comes and keeps ordering thousands of empty cups of coffee, clogging up the entire system and preventing real, paying customers from getting their caffeine fix. That's a DoS attack in a nutshell! 

This are three checklist items:
- Is the withdrawal pattern followed to prevent denial of service?

  **Description1**: To prevent denial of service attacks during withdrawals, it's critical to follow the withdrawal pattern best practices - pull based approach.

  _Remediation_: Implement withdrawal pattern best practices to ensure that contract behavior remains predictable and robust against denial of service attacks.

- Is there a minimum transaction amount enforced?
  
  **Description2:** Enforcing a minimum transaction amount can prevent attackers from clogging the network with zero amount or dust transactions.
    
  _Remediation:_ Disallow transactions below a certain threshold to maintain efficiency and prevent denial of service through dust spamming.

- How does the protocol handle blacklisting functionality tokens?

  **Description3:** Tokens with blacklisting capabilities, such as USDC, can pose unique risks and challenges to protocols.

  _Remediation:_ Account for the possibility of blacklisting within token protocols to ensure continued functionality even if certain addresses are blacklisted.

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

**Description4:** Forcing protocols to process queues, like a queue of dust withdrawals, can be exploited to cause a denial of service.

**Remediation:** Design queue processing in a manner that is resilient to spam and cannot be exploited to cause denial of service.

**Description5:** Tokens with low decimals can present issues where the transaction process fails due to rounding to zero amounts.

**Remediation:** Implement logic to handle low decimal tokens in a way that prevents the transaction process from breaking due to insufficient token amounts.

**Description6:** Protocols must handle interactions with external contracts in a way that does not compromise their functionality if external dependencies fail.

**Remediation:** Ensure robust handling of external contract interactions to maintain protocol integrity regardless of external contract performance.


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

Read (for more detailed explaination): https://www.cyfrin.io/blog/solodit-checklist-explained-3-donation-attacks 


------------------------------------------------- 

## (4) Front-Running Attacks

Front-Running Attacks: To illustrate, imagine you're at a busy farmers market where prices change based on demand. You spot an amazing deal on rare truffles for $50 that you know are worth $100 elsewhere. As you walk up to buy, a market insider who can see all incoming customer orders notices your intention. They quickly jump ahead of you in line, buy the truffles for $50, and then immediately offer to sell them to you for $90. You still get the truffles, but now this middleman has pocketed $40 of value that would have been yours.That's front-running in a nutshell - seeing someone else's pending transaction in a public system and then inserting your own transaction ahead of it to profit from the price change you know is coming.

In blockchain, this happens when attackers exploit the transparent nature of the mempool (where pending transactions wait to be mined, like packages waiting for delivery) to see the upcoming transactions. They then craft their own transactions with higher gas fees, ensuring theirs are executed first. This can lead to devastating consequences, from manipulated prices in decentralized exchanges (DEX) to stolen non-fungible tokens (NFTs).

**Check1:** Are 'get-or-create' patterns protected against front-running attacks?
  
 - Description: Functions combining resource creation and interaction (like getOrCreateAndUse) are vulnerable to front-running attacks where attackers can create the resource with different parameters before the victim, potentially manipulating prices or conditions.
   
 - Remediation: Separate creation and interaction into distinct transactions or implement robust protections (parameter validation, relative references instead of absolute values) to ensure safe operation regardless of creation timing.

**Check2:** Are two-transaction actions designed to be safe from frontrunning?
  
 - Description: Actions that require two separate transactions may be at risk of frontrunning, where an attacker can intervene between the two calls.
   
 - Remediation: Ensure critical actions that are split across multiple transactions cannot be interfered with by attackers. This can involve checks or locks between the transactions.

**Check3:** Can users maliciously cause others' transactions to revert by preempting with dust?
  
 - Description: Attackers may cause legitimate transactions to fail by front-running with transactions of negligible amounts.
   
 - Remediation: Implement checks to prevent transactions with non-material amounts from affecting the contract's state or execution flow.

**Check4:** s the protocol using a properly user-bound commit-reveal scheme?
  
 - Description: Sensitive on-chain actions can be exposed in the mempool, enabling frontrunning and information exploitation. Effective commit-reveal schemes must bind commitments to specific users and transactions.
   
 - Remediation: Implement a two-phase process where users first commit a hash containing their address and all transaction parameters, then reveal actual actions after the commitment phase ends, preventing frontrunning and information leakage.

**Conclusion**
Front-running attacks, like those potentially enabled by flawed commit-reveal schemes, are significant threats inherent to transparent blockchain environments. Because pending transactions often reside in a public mempool before confirmation, malicious actors can observe intentions and strategically submit their own transactions, often with higher gas fees, to execute before the target transaction for their own gain. This ability to observe and react before final execution is a unique challenge presented by the nature of public blockchains. Therefore, when developing or auditing any smart contract system, it is absolutely critical to adopt an adversarial mindset, particularly regarding timing and information availability:
‍
Always ask: "Can any function or interaction within this protocol be profitably front-run?"

Consider: What potentially sensitive information (like bids, trade details, price data, governance votes) is revealed before an action is immutably settled on-chain?

Analyze: What specific benefit could an attacker get by executing a transaction immediately before or after a specific user action? Could they capture arbitrage, manipulate prices, steal rewards, or censor others?
‍
Implement appropriate countermeasures, such as using secure commit-reveal patterns where necessary or minimizing dependencies on predictable external events. This is how developers build more resilient and trustworthy systems. Thinking like an attacker to anticipate vulnerabilities is fundamental to designing robust and secure smart contracts. 

Read (for more detailed explaination): https://www.cyfrin.io/blog/solodit-checklist-explained-4-front-running-attacks 

------------------------------------------------- 

## (5) Griefing Attacks

In a griefing attack, a malicious actor intends to disrupt or prevent legitimate users from executing desired functions. The attacker often incurs a cost (like gas fees) without gaining direct financial benefit from the disruptive action.

The term "griefing" likely originates from online gaming communities, where it describes players who intentionally irritate and harass others, often breaking the game's intended flow for their amusement rather than strategic advantage. Similarly, in the context of smart contracts, griefing attacks prioritize disruption and annoyance over profit. It's worth noting that within the web3 security space, the terms "griefing" and "denial-of-service (DoS)" are sometimes used interchangeably. 

A DoS attack is a term rooted in general network and computer security. It aims to make a service or a network unavailable to all users temporarily or indefinitely. This might involve overwhelming the system with traffic or exploiting vulnerabilities that prevent legitimate access for everyone. The goal is typically widespread disruption of the entire service. So, the scope and intent are the primary differentiators between the two attack types. However, the line can sometimes blur, which means some vulnerabilities might fit into either category. An impactful griefing attack could potentially lead to a DoS scenario for a subset of users or features.

**Check1:** Is there an external function that relies on states that can be changed by others?
  
 - Description: Malicious actors can prevent regular user transactions by making a slight change to the on-chain states.
   
 - Remediation: Ensure normal user actions, especially important actions like withdrawal and repayment, are not disturbed by other actors.

**Check2:** Can contract operations be manipulated with precise gas limit specifications?
  
 - Description: Attackers can supply carefully calculated gas amounts to force specific execution paths in the contract, manipulating its behavior in unexpected ways.
   
 - Remediation: Implement explicit gas checks before critical operations.

**Conclusion:**

Griefing attacks highlight that not all blockchain exploits aim for direct theft. Sometimes, the goal is simply disruption. By understanding the griefer's willingness to incur costs to inconvenience or block others, we can better anticipate vulnerabilities. Developing secure contracts requires an adversarial perspective:

_Always ask_: Can one user's action, even if seemingly irrational or costly, prevent another legitimate user from using the protocol as intended?

_Interrogate state changes_: Who controls the critical state? Can it be changed in a way that blocks others unfairly?

_Validate external interactions_: Does my contract assume success for external calls? What happens if they fail (due to out-of-gas or other reasons)? Is the state updated only after necessary external operations are confirmed successful?
By asking these questions and leveraging checklists like Solodit, developers can build more robust systems that are resistant to griefing attacks.



------------------------------------------------- 

## (6) Miner Attacks - Miner influence in blockchain systems

Miners possess several protocol-granted capabilities that directly impact blockchain operation. They determine transaction inclusion and ordering within blocks based on fee incentives (gas prices in Ethereum), giving them control over the mempool processing queue. This ordering power enables the profit capture of maximal extractable value (MEV) through strategic transaction positioning without violating consensus rules. Miners can manipulate block timestamps within network-defined tolerances, typically ±15 seconds in Ethereum and up to 2 hours in Bitcoin, which can potentially affect time-dependent contract logic, such as unlocking periods or interest calculations.

While other interesting attack vectors exist, including:

- 51% attacks: Controlling the majority of the hashrate to enable double-spending.
‍
- Selfish mining: Strategically withholding blocks to increase relative rewards.
‍
- Timejacking: Manipulating network time perception.
‍
- Eclipse attacks: Isolating nodes from honest peers)

Yet, these typically require either substantial computational resources or sophisticated network control mechanisms beyond standard mining operations. Such attacks target consensus-layer vulnerabilities rather than application-layer contract exploits.

**Check1:** Is block.timestamp used for time-sensitive operations?
  
 - Description: Miners can manipulate block.timestamp by several seconds, potentially affecting time-dependent contract logic.
   
 - Remediation: Use block.number instead of timestamps for critical timing operations, or ensure that manipulation tolerance is acceptable.

**Check2:** Is the contract using block properties like timestamp or difficulty for randomness generation?
  
 - Description: Block properties (timestamp, difficulty) and other predictable values should not be used for randomness as they can be influenced or predicted by miners.
   
 - Remediation: Use a secure randomness source, such as Chainlink VRF, commit-reveal schemes, or a provably fair randomization mechanism, instead.

**Check3:** Is contract logic sensitive to transaction ordering?
  
 - Description: Miners control transaction ordering and can exploit this for front-running, back-running, or sandwich attacks.
   
 - Remediation: Implement protection by allowing users to specify acceptable results that revert transactions when breached.

‍**Key takeaways:**

- block.timestamp is not a reliable time source. Use block.number or external oracles for critical timing operations.
‍
- Never use block properties for randomness. Opt for secure randomness sources like Chainlink VRF.
‍
- Be mindful of transaction ordering. Implement mitigation strategies like slippage protection.
------------------------------------------------- 


