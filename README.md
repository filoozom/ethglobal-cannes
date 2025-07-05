# Hackathon project

This project isn't built from scratch, but rather aims at contributing to the Clear Signing efforts led by Ledger. It contains 3 parts:

- [Hackathon project](#hackathon-project)
  - [Changes](#changes)
  - [Feedback](#feedback)
    - [Builder](#builder)
    - [ERC-7730](#erc-7730)
  - [Schemas](#schemas)

## Changes

As this is a hackathon project, there are no clearly defined pull requests or commits, just a branch that contains a bulk of the work. The diff can be found [here](https://github.com/filoozom/clear-signing-erc7730-builder/compare/main..hackathon) and the final version is in the [clear-signing-erc7730-builder submodule](clear-signing-erc7730-builder).

The main objective was to implement AI (and userdoc) suggestions for all the data, including metadata, operation names and field names. An LLM (hosted on OpenRouter) is fed all the information from Sourcify (devdoc, userdoc, sources, abi, etc) with the objective of allowing it to understand the contracts. A new tab was also added to the home page to find contracts (name and address) by protocol name.

## Feedback

This is rough unorganized feedback from using the builder.

### Builder

- There should be a boolean field format, unless I missed a way to achieve that.
- The UI shows `pure` and `view` functions. To me this doesn't make any sense, as they'd never get executed by a Ledger.
- DeFi protocols are often quite complex in their input data. It can be raw bytes that need specialized deserialization or data regular users wouldn't understand. Maybe some guidelines to explain how precise the field and operation names have to be would be useful.
- There should definitely be an option to import existing ERC-7730 JSON files, either as an upload or directly from the registry.
- It's a detail, but there should be a button to go back to the home page to work on a new contract (ideally with an exit warning prompt).
- It's not immediately clear how an array value is displayed on the Ledger. The builder shows a single value instead of a list:

    ```
    Amounts to loan for each token
    0.19866144 ETH
    ```

    should be something like:

    ```
    Amounts to loan for each token
    - 0.19866144 ETH
    - 0.19866144 ETH
    - 0.19866144 ETH
    ```

    or ideally even:

    ```
    Loans:
    - 10 DAI
    - 25 USDC
    ```

    Maybe this works better with arrays of tuples, which presumably have a different screen per array element? In any case, this is also not shown in the builder.

### ERC-7730

- Some way to describe more complex data would be nice, but probably difficult as they'd like require code to deserialize bytes.


## Schemas

Can be found in the [registry](registry) folder.
