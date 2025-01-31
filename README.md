# Bitcoin-Stacks Bridge Smart Contract

A secure and robust smart contract implementation that enables cross-chain transfers between Bitcoin and Stacks networks.

## Overview

The Bitcoin-Stacks Bridge smart contract facilitates secure asset transfers between the Bitcoin and Stacks blockchains through a validator-based system. It implements comprehensive security measures, including multi-signature validation, deposit confirmations, and emergency controls.

## Features

- **Cross-Chain Transfers**: Secure transfer of assets between Bitcoin and Stacks networks
- **Validator System**: Multi-validator architecture for enhanced security
- **Deposit Management**: Structured handling of deposits with confirmation requirements
- **Withdrawal System**: Secure withdrawal process to Bitcoin addresses
- **Emergency Controls**: Safety mechanisms including pause functionality and emergency withdrawals
- **Balance Management**: Accurate tracking of user balances and total bridged amounts

## Contract Components

### Constants

- `MIN_DEPOSIT_AMOUNT`: 100,000 (minimum allowed deposit)
- `MAX_DEPOSIT_AMOUNT`: 1,000,000,000 (maximum allowed deposit)
- `REQUIRED_CONFIRMATIONS`: 6 (required confirmations for deposit processing)

### Error Codes

| Code | Description               |
| ---- | ------------------------- |
| 1000 | Not authorized            |
| 1001 | Invalid amount            |
| 1002 | Insufficient balance      |
| 1003 | Invalid bridge status     |
| 1004 | Invalid signature         |
| 1005 | Already processed         |
| 1006 | Bridge paused             |
| 1007 | Invalid validator address |
| 1008 | Invalid recipient address |
| 1009 | Invalid BTC address       |
| 1010 | Invalid transaction hash  |
| 1011 | Invalid signature format  |

## Core Functions

### Administrative Functions

```clarity
(define-public (initialize-bridge))
(define-public (pause-bridge))
(define-public (resume-bridge))
(define-public (add-validator (validator principal)))
(define-public (remove-validator (validator principal)))
```

### Bridge Operations

```clarity
(define-public (initiate-deposit
    (tx-hash (buff 32))
    (amount uint)
    (recipient principal)
    (btc-sender (buff 33))
))

(define-public (confirm-deposit
    (tx-hash (buff 32))
    (signature (buff 65))
))

(define-public (withdraw
    (amount uint)
    (btc-recipient (buff 34))
))

(define-public (emergency-withdraw
    (amount uint)
    (recipient principal)
))
```

### Read-Only Functions

```clarity
(define-read-only (get-deposit (tx-hash (buff 32))))
(define-read-only (get-bridge-status))
(define-read-only (get-validator-status (validator principal)))
(define-read-only (get-bridge-balance (user principal)))
```

## Security Features

1. **Validator Authentication**: Only authorized validators can initiate and confirm deposits
2. **Amount Validation**: Strict checks on deposit amounts within defined limits
3. **Multi-signature Requirements**: Multiple validator signatures required for deposit confirmation
4. **Address Validation**: Comprehensive validation for both Bitcoin and Stacks addresses
5. **Transaction Uniqueness**: Prevention of duplicate transaction processing
6. **Emergency Controls**: Ability to pause bridge and perform emergency withdrawals
7. **Balance Protection**: Strict balance checking before withdrawals

## Usage Flow

### Deposit Process

1. User initiates Bitcoin transaction
2. Validator calls `initiate-deposit` with transaction details
3. Required number of validators confirm deposit using `confirm-deposit`
4. Upon sufficient confirmations, funds are credited to recipient

### Withdrawal Process

1. User calls `withdraw` with amount and Bitcoin recipient address
2. Contract verifies user balance and amount validity
3. Upon successful validation, withdrawal event is emitted
4. Validators process the withdrawal on Bitcoin network

## Error Handling

The contract implements comprehensive error handling with specific error codes for different scenarios. All functions return a response type that includes either success confirmation or an error code.

## Data Storage

### Maps

- `deposits`: Stores deposit information
- `validators`: Tracks authorized validators
- `validator-signatures`: Records validator signatures for deposits
- `bridge-balances`: Maintains user balances

### Variables

- `bridge-paused`: Current bridge status
- `total-bridged-amount`: Total amount currently bridged
- `last-processed-height`: Last processed block height

## Security Considerations

1. **Never share private keys** or validator credentials
2. Always verify transaction hashes and signatures
3. Monitor bridge status before initiating transactions
4. Ensure proper validator management
5. Verify recipient addresses carefully

## Best Practices

1. **Validation**: Always validate inputs before submitting transactions
2. **Confirmations**: Wait for required confirmations before considering deposits final
3. **Amount Limits**: Stay within defined deposit limits
4. **Address Format**: Ensure correct formatting of Bitcoin and Stacks addresses
5. **Error Handling**: Properly handle all potential error responses

## Emergency Procedures

In case of security concerns:

1. Contract owner can pause the bridge using `pause-bridge`
2. Emergency withdrawals can be performed using `emergency-withdraw`
3. Validators can be removed if compromised using `remove-validator`

## Limitations

- Maximum deposit amount: 1,000,000,000 units
- Minimum deposit amount: 100,000 units
- Required confirmations: 6
- Only supports standard Bitcoin addresses
- Emergency withdrawals restricted to contract deployer

## Contributing

When contributing to this contract:

1. Ensure all new functions include proper validation
2. Add appropriate error codes for new failure cases
3. Update documentation for any changes
4. Include comprehensive tests for new functionality
5. Follow existing code style and conventions
