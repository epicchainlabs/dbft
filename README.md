

![Codecov](https://img.shields.io/codecov/c/github/epicchainlabs/epicchain.svg)
[![Report](https://goreportcard.com/badge/github.com/epicchainlabs/epicchain)](https://goreportcard.com/report/github.com/epicchainlabs/epicchain)
![License](https://img.shields.io/github/license/epicchainlabs/epicchain.svg?style=popout)

# EpicChain

Welcome to the **EpicChain** repository! This project contains the implementation of the EpicChain blockchain, including its consensus algorithm, models, and related components. EpicChain aims to deliver a robust and scalable blockchain infrastructure with high performance and flexibility, providing a solid foundation for decentralized applications and smart contracts.

## Overview

The EpicChain project encompasses various critical elements necessary for building and maintaining a blockchain network. Our implementation focuses on providing a secure and efficient framework for consensus, data integrity, and interoperability. Below, you will find a detailed breakdown of the design and structure of the EpicChain implementation.

## Design and Structure

1. **Core Package**:
   - The core control flow is managed in the main `epicchain` package. This package is designed with high flexibility and extendability in mind. It abstracts external communication and event handling behind interfaces, callbacks, and generic parameters. Detailed descriptions of configuration options can be found in the `config.go` file, enabling you to customize and adjust settings according to your needs.

2. **Cryptography**:
   - The `epicchain` package includes interfaces for `PrivateKey` and `PublicKey`, allowing developers to integrate their own cryptographic solutions for signing blocks during the commit stage. For more information on these interfaces, refer to `identity.go`. Note that no default cryptographic implementations are provided, giving you the freedom to choose or implement your preferred cryptographic methods.

3. **Hashing**:
   - Similarly, the `epicchain` package provides a `Hash` interface that enables the use of custom hashing algorithms without additional overhead for conversions. You can instantiate EpicChain with a custom hash implementation that aligns with your requirements. Details about the `Hash` interface are available in `identity.go`, and again, no default implementation is provided.

4. **Abstractions**:
   - The `epicchain` package includes `Block` and `Transaction` abstractions, defined in `block.go` and `transaction.go`. These abstractions ensure that every block can be signed, verified, and accessed for essential fields. Transactions are entities that can be hashed, and entities with matching hashes are considered equivalent. No default implementations are provided, allowing for customization according to your project's needs.

5. **Payloads**:
   - The repository also contains generic interfaces for payloads, but no default implementations are provided. This design allows for flexible payload handling suited to various use cases.

6. **Timers**:
   - A `Timer` interface is included for managing time-related operations. The `timer` package offers a default `Timer` provider suitable for production environments. The interface is primarily designed for testing scenarios that involve time-dependent behavior in EpicChain.

7. **Custom Implementations**:
   - The `internal` directory features examples of custom identity types and payload implementations, used to demonstrate the application of EpicChain with a 6-node consensus model. You can find type-specific implementations and tests in the `internal` subpackages. The `internal/simulation` folder provides an example of how to use the EpicChain library effectively.

8. **Formal Models**:
   - The `formal-models` directory contains a set of models for EpicChain written in [TLA⁺](https://lamport.azurewebsites.net/tla/tla.html) language. These models are accompanied by instructions on how to run and verify them. For more details, refer to the [README](./formal-models/README.md) in the `formal-models` directory.

## Usage

To utilize the EpicChain library, you must implement your own event loop. The library provides five critical callbacks that influence the state of the consensus process:

- **`Start()`**: Initializes the internal EpicChain structures.
- **`Reset()`**: Reinitializes the consensus process to handle a new height or state.
- **`OnTransaction()`**: Should be called whenever a new transaction appears, allowing the system to process and include it in the blockchain.
- **`OnReceive()`**: Should be invoked every time a new payload is received, ensuring that the system processes incoming data.
- **`OnTimer()`**: Must be called whenever a timer event occurs, managing time-related tasks within the consensus process.

A minimal working example is available in `internal/simulation/main.go`, demonstrating basic usage and setup of the EpicChain library.

## Additional Resources

For further information and in-depth understanding, you can explore the following resources:

- **EpicChain Consensus Overview**: A high-level description of the consensus mechanism used in EpicChain. [Learn more](https://docs.epicchain.org/consensus)
- **EpicChain Research Paper**: Detailed documentation and research paper outlining the theoretical foundation and design principles of EpicChain. [Read the paper](https://github.com/EpicChainResearch/whitepaper)

## Notes

1. **Memory Pool Model**:
   - The EpicChain implementation supports a memory pool model, where transaction hashes are proposed initially, and transactions are synchronized in the background. Some callbacks are configured to support this use case, but it is easy to extend `PrepareRequest` to include proposed transactions if needed.

2. **Validator Management**:
   - EpicChain includes functionality for managing validators, which are responsible for verifying blocks. The `GetValidators` callback is called at the start of each epoch to retrieve the list of validators. In scenarios where validators are constant, this callback can return the same value consistently.

3. **Block Processing**:
   - The `ProcessBlock` callback is invoked synchronously whenever a new block is accepted. It is responsible for persisting the block and updating the blockchain state if required. The library does not automatically initialize EpicChain for the next height after block collection; this responsibility lies with the caller, who must ensure proper initialization and state updates before proceeding.

Feel free to explore the repository, and don't hesitate to reach out to the EpicChain community if you have any questions or need further assistance with your implementation.
