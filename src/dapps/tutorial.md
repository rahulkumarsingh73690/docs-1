# Tutorial

In this tutorial, we will set up a local testnet environment for smart contract development, and help find your bearings by writing, uploading, and interacting with your first smart contract. By the end of this tutorial, you'll have a complete working dApp that works with your custom token issued on the Terra blockchain.

## Setting up your environment

In order to work with Terra Smart Contracts, you should have access to a Terra network that includes the WASM integration. As a smart contract developer, you will need to write, compile, upload, and test your contracts before deploying them to be used on the Columbus mainnet. As this development process can involve manually juggling multiple moving parts over many iterations, it is helpful to first set up a specialized environment to streamline development.

**localterra** is a package that includes everything you need to work with smart contracts on your local machine. It spins up a local, WASM-enabled private testnet with a single validator submitting Oracle votes and includes a version of FCD, Station and Finder. 