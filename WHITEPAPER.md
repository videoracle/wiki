# **VideOracle Whitepaper**
**A protocol to verify real-world events on-chain through video**

[**_@codethazine_**](https://github.com/codethazine)_ and _[**_@thundering-silence_**](https://github.com/thundering-silence)_

## **Abstract**

Currently, real-world events cannot be easily verified in a reliable manner. We are designing a protocol to verify events in a decentralized manner through video proofs. This method revolves around three parties (requesters, verifiers, and voters) to reach a consensus over real-world events.


## **Background & Motivation**

### **Verifying real-world events**

Today businesses and organizations are suffering from a lack of transparency over the verification of real-world events.

Brands may want to verify that the shops execute in-store marketing as per requirements. Town councils and other public offices may wish to involve citizens in proving squatters have taken over abandoned buildings or that road works are being correctly executed. Individuals could desire proof of the existence or state of an Airbnb before booking, hence avoiding unpleasant surprises.

VideOracle sets out to solve these issues through a decentralized and trustless verification protocol. The protocol's architecture is detailed below.



## **Participants**

We designed VideOracle to have a reliable and trustless mechanism for verifying real-world events. This is achieved by decentralizing the process of verification and spreading responsibilities over different parties.

### **Requester**

Requesters are parties that submit verification requests.

A _Request_ will have the following attributes:

1. Body - explaining what the request aims to verify.
   E.g., _Are Vyper shoes on display in the main window shop?_
2. Location - coordinates of where the video request should be taken.
   E.g., _45.46314473546967, 9.187684057670879_
3. Answer Type – the type of answer expected between a value in a set, number, or boolean.
   E.g., _Boolean_
4. Reward - amount to be rewarded to users for successfully answering the query.
   E.g., _150 MATIC_
5. Deadline - timestamp after which the request is considered expired.
   E.g., _5 days_
6. Minimum Voters - number of votes needed for a verification video to be considered reliable.
   E.g., _3 voters_

The reward will have to be provided upfront.

When the deadline is over, the requester will then have to either accept the verification (which will automatically distribute the reward) or reject it and open a dispute - more on this later.

### **Verifiers**

Verifiers are parties that submit verification videos to requests.

To verify a request, verifiers need to:

1. Record the video verification
2. Submit the video verification to the protocol

Each verifier can submit only one video per request.

If the verification video was selected as the winning one, 50% of the reward will be awarded.

### **Voters**

Voters are responsible for electing the video that best answers the request.

To vote for a verification video, voters need to:

1. Select the request on which they want to vote on
2. Stake an amount of tokens equal to the request's reward divided by the minimum number of votes required. When the minimum number of votes is passed, this staking value is going to be re-computed to be equal to the request's reward divided by the current number of voters. Previous voters will be reimbursed for the staking difference
3. Submit the vote to a selected video

Each voter is allowed only one vote per request.

If the voted video wasn’t selected as the winning one, the stake is returned to the user. 

On the other hand, if the voted video was selected as the winning one, 50% of the reward will be awarded (split among its voters), and the staked amount will be returned. 

In case of a dispute, users who had not voted for the disputed verification video will be called to judge the legitimacy of the dispute - more on this later.

## **Architecture**

![](https://lh4.googleusercontent.com/TLgGlxuMdJcoi2LioBakdaNK4wM7ItXOQ266fgxUaxA08TGPb7VHZUR1XdIf6M4x_5i0em6P97UDPHN1PLHk0EGoBaRnErxLWidt7Ot1MdNU_p5UJ-AnKBmj0aGN4y1JLOiDlm580xTdB1YSCS_0OSha-ncTJfICs6JWO7KnnBp5YQe5Ko4ctHqhzmN5vNW-)

#### **Phase 1. Requester makes request**

1. A request with the following attributes is made:

   1. Request body
   2. Request location
   3. Answer type
   4. MATIC Reward
   5. Minimum Voters
   6. Deadline

2. The request gets submitted to a smart contract on Polygon

#### **Phase 2. Verifier verifies the request**

1. The verifier records and posts their video
2. Answers the request with a value
3. The video gets hosted on IPFS and minted as an NFT on Polygon through the Livepeer SDK
4. The NFT is attached to the request on the Smart Contract

#### **Phase 3. Voter stakes before voting**

1. The voter selects a request
2. A staking value is computed, as equal to the request's reward divided by the minimum number of votes required. When the minimum number of votes is passed, this staking value is going to be re-computed to be equal to the request's reward divided by the current number of voters. Previous voters will be reimbursed for the staking difference
3. The voter stakes the computed amount in a staking pool with the rest of the voters

The staking re-computation is done to have the same amount of reward for voting in favor or not on the dispute and thus keep the voting incentive balanced.

#### **Phase 4. Voter votes on the verification video**

1. The voter selects a verification video
2. Submits the vote to the on-chain request

#### **Phase 5. Requester accepts or declines**

1. As soon as the _deadline_ is reached, the request is closed

2. Requester views the video and decides to accept or decline the verification
   a. In case the requester accepts the verification, the staked money gets returned to each voter and the reward gets divided in the following way: 
   
      1. 50% to the verifier who made the video
      2. 50% split among the voters who voted for the video

   b. In case the requester declines the verification, a dispute is opened:
   
      1. The requester stakes an amount equal to 10% of the reward
      2. The voters that verified all the other videos will be called to vote on the disputed case for 72 hours
      3. In case the dispute is resolved in favor of the requester, the money staked by the voters that voted for the disputed video will be split among the voters that voted in the dispute case and the requester gets his reward and stake back
      4. In case the dispute is resolved in disfavor of the requester, the money staked by the requester is split among the voters that voted in the dispute, the staked amount is returned to the voters, and the original reward proceeds to get split 50% to the verifier and 50% to the voters

In case the number of voters on other videos is lower than the minimum number of voters, the request is considered indisputable. In case no votes are given to the dispute for 72 hours, the dispute is considered resolved in favor of the requester.

In case the _deadline_ and the _minimum voters_ criteria hasn’t been met, the stake pool is returned to the voters and the reward is returned to the requester. In case two videos reach the same number of votes, the first one to be submitted chronologically wins.

## **Conclusion**

The protocol will be deployed to serve as the basis for verifying real-world events on top of the blockchain through video. As the verification requests grow in number, this data can then be used in other smart contracts to create new DApps with infinite use cases.
