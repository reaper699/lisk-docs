# Lisk Delegate Handbook

#### 8th March 2016

## Table of Contents

### A. Consensus
1. Delegated Proof of Stake (DPoS)
2. Benefits of Delegated Proof of Stake in comparison to PoW/PoS
3. What are the drawbacks of DPoS?

### B. Delegates
1. What is a delegate?
2. What is the difference between an active and a standby delegate?
3. What are delegate votes and where can I vote for delegates?
4. How can I become an active delegate?
5. Can I run multiple delegates (from the same IP, computer..)?
6. How important is the uptime of a delegate and how is it calculated?
7. What is a delegate round?
8. How long does it take until my votes appear on the network?
9. In what order are the active delegates forging the blocks?

### C. Forging
1. What is forging?
2. Who can forge?
3. How can I enable forging as a delegate?
4. Do I have to update to the latest version if I want to forge?
5. What are the recommended server specifications for forging?
6. How much can I earn by forging and how are the fees distributed?
7. Does it makes a difference how many LSK my delegate account holds?
 
### A. Consensus
**1. Delegated Proof of Stake (DPoS)**

Proof of Delegates is the consensus algorithm used by Lisk. It combines elements taken from the Proof of Work (PoW), Proof of Stake (PoS), and Delegated Proof of Stake (DPoS) consensus algorithms.
 
The Lisk network is secured and/or protected by 101 active delegates. Each delegate is elected by the stakeholders of LSK. Once voted into the list of active delegates they are given the authority to generate blocks. Every Lisk stakeholder can be a part of the electoral process, by placing votes for delegates in their favour, or by becoming a candidate themselves.  
The duty of the 101 active delegates is to secure the Lisk main blockchain (i.e. the mainchain). In order to provide an incentive to secure the network, transaction fees on the network are distributed equally amongst the 101 active delegates. In addition, an inflationary block reward (aka forging reward) is distributed to each block generator.
 
**2. Benefits of Delegated Proof of Stake in comparison to PoW/PoS**
 
Every blockchain based crypto-currency to date, uses a consensus algorithm in order to determine who will generate the next block.
 
Bitcoin introduced Proof of Work (PoW) from which miners have to calculate a solution (hash) to a problem, wherein the more powerful the miner is the faster he can find that solution. The miner who finds the solution first, mines (generates) the block, and a specific amount of Bitcoin along with it. That means the miner has a financial incentive to be the fastest. Therefore, Bitcoin has an ongoing arms race to become the most powerful miner, leading to an ever increasing energy consumption of the whole network. 

Nxt introduced Proof of Stake (PoS) which removed the mining element from the network, thus reducing the energy consumption dramatically. With Nxt, your stake, i.e. the amount of coins you own, determines your chance to forge (generate) the next block. Therefore, there is not an arms race as such, but rather an emphasis on increasing your NXT stake, in order to increase your forging chance. However, in comparison to Lisk, the Nxt forging rewards are negligible.
 
Lisk combines both worlds; the energy friendly Proof of Stake (PoS) algorithm, with the competitive element of Proof of Work (PoW). Additionally, Lisk adds a community element of elected delegates to the consensus mechanism.

**3. What are the drawbacks of DPoS?**
 
Having a fixed amount of active delegates may result in a less decentralized network. Ideally, the community aspect of the network should fix that issue. If only a small part of the Lisk users vote for delegates this problem becomes worse. In the future the number of active delegates can also be increased, if the demand for it is there.

The DPoS variant of Lisk also introduced inflationary forging rewards in order to have an incentive to become a delegate. While this is no real drawback some might have concerns over an inflationary crypto-currency.
 
Daniel Larimer, founder of BitShares and DPoS, argues that 101 single-purpose servers are more decentralized (and cheaper) than a thousand or so miners on a few mining pools. [1]
 
>Already, Bitcoin is centralized to the point that with just three mining pools, you can control 51% of the network. With just four ASIC chip manufacturers, you can control 90%+ of production of future hashing power.”
 
Vitalik Buterin, founder of Ethereum, calls our type of blockchain a consortium blockchain. [2]
 
>I think it is incorrect to say that [a consortium blockchain] is centralized [..]. Realistically, it is secure enough; it is decentralized enough. And it does provide fairly strong security assurances by itself.
 
### B. Delegates
**1. What is a delegate?**
 
A delegate is nothing more than a special type of Lisk account. Any Lisk account can become a delegate, by simply registering a delegate username within the client. After the registration your account ID appears in the list of all delegates. The registration fee is 100 LSK, however this may be subject to change in the future.
 
**2. What is the difference between an active and a standby delegate?**
 
Every delegate is placed at a specific position on the delegate ranking list. The number of votes determines that position. All delegates with a rank between 1 and 101 are active delegates. All other delegates with a rank over 101 (102-∞) are classified as standby delegates.
 
**3. What are delegate votes and where can I vote for delegates?**
 
In order to determine the delegate rank position, Lisk has a decentralized voting mechanism built directly into the client. Users can vote for any delegates registered on the network. One vote equals 0.00000001 LSK, and a user can only vote with his entire LSK balance. One vote costs the user 1 LSK and he can vote for 33 delegates in one go. He can vote for 101 delegates in total, for this he needs to initiate 4 votes (33+33+33+2 = 101). It is not possible to vote for the same delegate twice.
 
The number of votes are represented as an “Approval” within the client, and is shown as a percentage. An approval of 1% equals 1% of all LSK in the network. At launch this would be 1,000,000 LSK (later more, due to inflation) or 100,000,000,000,000 votes.
 
**4. How can I become an active delegate?**
 
In order to become an active delegate, you must attain a higher approval percentage than the delegate on rank 101. This means you will overtake them in the ranking list and become the 101st delegate, and therefore, become one of the 101 active delegates.
 
**5. Can I run multiple delegates (from the same IP, server..)?**
 
Yes. However, this is not recommended and if it’s public knowledge, it should ideally be frowned upon by the community by removing the votes from both delegates.

The other way around, maintaining multiple servers for the same active delegate is not recommended! Your servers will get on a fork.
 
**6. How important is the uptime of a delegate and how is it calculated?**
 
Very important. The uptime is also visible as a percentage and doesn’t represent the uptime of the node, but rather the number of generated blocks by one delegate in relation to the number of blocks, which were possible to generate for the delegate.
 
That means if you became an active delegate 606 blocks ago, you had the chance to forge 6 blocks. If you only forged 5 blocks, because your node was unable to sign a given block, maybe because it was offline or not responding efficiently enough, you will have an uptime of 83.3%.
 
**7. What is a delegate round?**
 
A delegate round is exactly 101 blocks in length. If a delegate A can’t generate a block (e.g. their node is offline) they won’t be classed as a participant of that round, therefore another delegate B out of the active delegates takes his place (only for that round). Delegate B will then generate a total of 2 blocks in that round. Therefore, he will also receive twice the block reward amount, and a higher proportion of fees will be distributed to all other active delegates participating in the round.
 
In the case where a delegate is unable to generate the next block, the block time will lengthen to 20 seconds. If two delegates in a row are unable to generate the next block, the block time will then lengthen to 30 seconds.
 
In an ideal round where 101 delegates are online, one delegate round takes 101*10 sec = 1010 sec = 16.83 min.
 
In the worst case scenario, where only 1 delegate is online, one new block takes on average 50*10 sec = 500 sec = 8.3 min. That will mean one delegate round takes 101*8.3min = 11.97 hours.
 
**8. How long does it take until my votes appear on the network?**
 
It takes until the beginning of the next delegate round for your votes to appear on the network. If your vote is placed at the 100th block, then you will only need to wait until one new block is generated. Conversely, if your vote is placed at the 10th block, you will have to wait for 91 further blocks to be generated.
 
**9. In what order are the active delegates generating the blocks?**
 
In every new delegate round the order of delegates is random. With 101 delegates this results in a huge number of possible orders.
 
- 2 delegates: 2! or: 2 orders (12, 21)
- 3 delegates: 3! or 6 orders (123,132,213,231,312,321)
- 4 delegates: 4! or 24 orders
- ..
- 101 delegates: 101! or: 9425947759838359420851623124482936749562312794702543768327889353416977599316221476503087861591808346911623490003549599583369706302603264000000000000000000000000 orders

### C. Forging
**1. What is forging?**
 
Forging is another word for block generation, at Bitcoin this process is called mining. The term “forging” was coined by the Nxt community. Earlier PoS systems called the process “staking”.

**2. Who can forge?**
 
Everyone can enable forging, but only the 101 active delegates will actually forge and earn rewards.

**3. How can I enable forging as a delegate?**
 
There are two methods to enable forging.
 
You can login to the client user interface and enable forging by hand. The problem with this method is, that if your client restarts (due to an error or server update) forging will need to be enabled again.
 
For optimum uptime, it is recommended to run your own node and insert your passphrase into the config.json file. Best to do this before you become an active delegate. Upon starting the Lisk client, forging will be automatically enabled for all accounts for which passphrases are specified in the config.json. Which means after restarting your client, forging will continue without interruption.
 
To avoid putting the majority of your funds at risk by saving the passphrase on a server, we also recommend creating a Lisk account which only acts as a delegate, from which your forged Lisk can then be periodically transferred back to your master account.
 
**4. Do I have to update to the latest version if I want to forge?**
 
With each update we will specify if delegates need to update or not. If new features involve backend changes, you will likely have to update.
 
**5. What are the recommended server specifications for forging?**
 
The most important factor is your internet connection latency. Exact numbers will be revealed in the future. Right now we estimate, that any cloud hosting provider, and most modern home connections should be more than sufficient.
 
One other important factor is memory; we currently recommend at least 512MB memory. However, during testing, we have successfully started the Lisk client on host machines with as little as 64MB memory, providing your machine’s operating system has enough swap memory enabled.

**6. How much can I earn by forging and how are the fees distributed?**
 
In addition to the forging reward earned for every block your delegate generates, your delegate will also earn an equal share of all regular transaction fees occurring on the network.
 
The total share of regular transaction fees, is dependent on the volume of transactions occurring on the network for a given round.
 
The forging rewards occur at a fixed rate per block, and change over the course of the network’s lifetime as follows:
 
- 5 LSK per block - in the 1st year.
- 4 LSK per block - in the 2nd year.
- 3 LSK per block - in the 3rd year.
- 2 LSK per block - in the 4th year.
- 1 LSK per block – in all other years.

**7. Does it make a difference how many LSK my delegate account holds?**

No, it doesn’t. An active delegate with 100 LSK receives the same amount of LSK from forging as an active delegate with 1,000,000 LSK.

### D. Sources
[1] http://bytemaster.github.io/bitshares/2015/01/04/Delegated-Proof-of-Stake-vs-Proof-of-Work/  
[2] http://coinjournal.net/vitalik-buterin-on-misconceptions-in-the-private-vs-public-blockchain-debate/
