# Hackathon project

This project isn't built from scratch, but rather aims at contributing to the Clear Signing efforts led by Ledger. It contains 3 parts:

- [Hackathon project](#hackathon-project)
  - [Changes to clear-signing-erc7730-builder](#changes-to-clear-signing-erc7730-builder)
  - [Feedback about ERC-7730 and the builder](#feedback-about-erc-7730-and-the-builder)
    - [Builder](#builder)
    - [ERC-7730](#erc-7730)
  - [ERC-7730 schemas](#erc-7730-schemas)

## Changes to clear-signing-erc7730-builder

As this is a hackathon project, there are no clearly defined pull requests or commits, just a branch that contains a bulk of the work. The diff can be found [here](https://github.com/filoozom/clear-signing-erc7730-builder/compare/main..hackathon) and the final version is in the [clear-signing-erc7730-builder submodule](clear-signing-erc7730-builder).

The main objective was to implement AI (and userdoc) suggestions for all the data, including metadata, operation names and field names. An LLM (hosted on OpenRouter) is fed all the information from Sourcify (devdoc, userdoc, sources, abi, etc) with the objective of allowing it to understand the contracts. A new tab was also added to the home page to find contracts (name and address) by protocol name. Admittedly, the AI part is not currently very functional as it takes a long time to load. Ideally, this should be done in the background at some point. Experimenting with faster models could potentially work as well.

The updates also include an attempt of porting the [python-erc7730](https://github.com/LedgerHQ/python-erc7730) module [here](https://github.com/filoozom/clear-signing-erc7730-builder/blob/hackathon/src/server/api/lib/generate.ts), with the objective of having a unified codebase in the same language. However, due to time constraints this wasn't finished. The current implementation still uses the Python API and extends its output with additional data.

Another very useful function I added is to import existing schemas to continued editing them. This was done in commit [f2b56e1](https://github.com/filoozom/clear-signing-erc7730-builder/commit/f2b56e10b7f134d40c2e94798ad8361e2dc007f6).

## Feedback about ERC-7730 and the builder

This is rough unorganized feedback from using the builder.

### Builder

- There should be a boolean field format, unless I missed a way to achieve that.
- The UI shows `pure` and `view` functions. To me this doesn't make any sense, as they'd never get executed by a Ledger.
- DeFi protocols are often quite complex in their input data. It can be raw bytes that need specialized deserialization or data regular users wouldn't understand. Maybe some guidelines to explain how precise the field and operation names have to be would be useful.
- There should definitely be an option to import existing ERC-7730 JSON files, either as an upload (implement in [f2b56e1](https://github.com/filoozom/clear-signing-erc7730-builder/commit/f2b56e10b7f134d40c2e94798ad8361e2dc007f6)) or directly from the registry.
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


## ERC-7730 schemas

Created with the use of the AI-augmented builder. Can be found in the [registry](registry) folder.
