## Oasis Governance powered by the blockchain

The Oasis Community Governance will allow anyone with a Oasis wallet to submit project proposals for integrations, promoting, new exchanges, marketing, dev, etc and get the requested OASIS coins if more than a net of 10% of MasterNode owners choose to support it with **Yes** votes.
### Create a proposal

All the commands assume a CLI wallet.

You can run them in the debug console of a Qt wallet as well, just remove the `$ oasis-cli ` part from the commands.

### Identify the next budget superblock to pin the proposal to:

```
$ oasis-cli getnextsuperblock
432000
```

### Create an address in the wallet that will receive the funds if the proposal is voted Yes by the MasterNodes:
```
$ oasis-cli getaccountaddress "proposal1"
oev6kaChaphBj8iKsg4U2srEh5rYjtgLNW
```

### Create the proposal using the superblock and address from above. 

In this example we are asking for 30 OASIS coins. Min amount that can be requested is `2`. Value `1` after the URL indicates that this is a one-off proposal, targeted to be paid on block 432000 if more than 10% net masternodes support it. Proposal name, `test1` in this example, is limited at 20 characters and URL at 64. The URL is there to provide additional details about the proposal so that MN owners can decide if they should vote it yes or no.
```
$ oasis-cli preparebudget "test1" "https://github.com/OasisCoinTeam/Proposals/issues/1" 1 432000 "oev6kaChaphBj8iKsg4U2srEh5rYjtgLNW" 30
98dc3276c7f3bae05b3f4f1a60085f0903a3731a8f5ee4f6a0fe1ad24d206bc8
```

Use the help subcommand if you want more details about each parameter:
```
oasis-cli help preparebudget
```

### Wait for the 2 OASIS coins proposal fee transaction to gain 6 confirmations
```
$ oasis-cli gettransaction 98dc3276c7f3bae05b3f4f1a60085f0903a3731a8f5ee4f6a0fe1ad24d206bc8
```

### Once we have 6 confirmations for the proposal fee, we can submit the proposal
```
$ oasis-cli submitbudget "test1" "https://github.com/OasisCoinTeam/Proposals/issues/1" 1 432000 "oev6kaChaphBj8iKsg4U2srEh5rYjtgLNW" 30 "98dc3276c7f3bae05b3f4f1a60085f0903a3731a8f5ee4f6a0fe1ad24d206bc8"
```

### See all proposals for this budget cycle
```json
$ oasis-cli getbudgetinfo
[
    {
        "Name" : "test1",
        "URL" : "https://github.com/OasisCoinTeam/Proposals/issues/1",
        "Hash" : "1c4c744a3b684b535b0eafe30a9c366e189554a53e9c76b542b9cbba8887f886",
        "FeeHash" : "98dc3276c7f3bae05b3f4f1a60085f0903a3731a8f5ee4f6a0fe1ad24d206bc8",
        "BlockStart" : 432000,
        "BlockEnd" : 475200,
        "TotalPaymentCount" : 1,
        "RemainingPaymentCount" : 1,
        "PaymentAddress" : "oev6kaChaphBj8iKsg4U2srEh5rYjtgLNW",
        "Ratio" : 0.00000000,
        "Yeas" : 0,
        "Nays" : 0,
        "Abstains" : 0,
        "TotalPayment" : 30.00000000,
        "MonthlyPayment" : 30.00000000,
        "IsEstablished" : true,
        "IsValid" : true,
        "IsValidReason" : "",
        "fValid" : true
    }
]
```

### Vote yes on the above proposal from the masternode CLI
```
$ oasis-cli mnbudgetvote local "1c4c744a3b684b535b0eafe30a9c366e189554a53e9c76b542b9cbba8887f886" yes
```

### Alternatively you can vote from the Cold wallet as well on behalf of multiple masternodes
```
$ oasis-cli mnbudgetvote many "1c4c744a3b684b535b0eafe30a9c366e189554a53e9c76b542b9cbba8887f886" yes
```

### View the status of your proposal

In a few minutes, your proposal should be visible on [http://oasis.mn.zone/proposals](http://oasis.mn.zone/proposals). Time to lobby your newly added proposal, welcome to politics!