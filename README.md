# Quantum Grover Algorithm for Azure Blob Storage

This repository contains a Quantum Grover algorithm implemented in Q# and integrated with Azure Blob Storage. The algorithm is designed to search for a marked item in an unsorted Azure Blob Storage, using Grover's quantum search algorithm.

## Prerequisites

Before running the quantum algorithm, ensure you have the following:

- **Azure Quantum Workspace**: Create an Azure Quantum Workspace using the [Azure Portal](https://portal.azure.com/).

- **Azure Storage Account**: Set up an Azure Storage Account with a Blob Container where the unsorted data is stored.

- **Quantum Development Kit (QDK)**: Install the Quantum Development Kit, which includes the Q# compiler and runtime. You can download it from [here](https://learn.microsoft.com/en-us/azure/quantum/).

## Quantum Algorithm Overview

The Quantum Grover algorithm is designed to search for a marked item in an unsorted database. In this example, the marked item is represented by the state |1⟩. The algorithm uses Grover's iterations to amplify the amplitude of the marked state, increasing the probability of measuring the marked item.

## Q# Code Breakdown

### Marking Oracle

The `MarkingOracle` operation in Q# flips the sign of the amplitude of the marked state. This is where the quantum search condition is applied.

```qsharp
operation MarkingOracle(qubits : Qubit[]) : Unit is Adj {
    // Apply the oracle logic to mark the solution states.
    // This logic depends on the specific structure of your database and how you encode your items.
    // For simplicity, let's assume the marked item is |1⟩.
    X(qubits[0]);
}
```

### Diffusion Operator

The `DiffusionOperator` operation implements the Grover diffusion operator, which amplifies the amplitude of the desired state.

```qsharp
operation DiffusionOperator(qubits : Qubit[]) : Unit is Adj {
    H(qubits);
    ApplyPauliXor([PauliX(q) | q in qubits], qubits);
    H(qubits);
}
```
### Grover`s Algorithm

The `GroverAlgorithm` operation runs Grover's iterations to search for the marked item.

```qsharp
operation GroverAlgorithm(numIterations : Int, markedItem : Int, qubits : Qubit[]) : Unit {
    // Initialize the qubits.
    ApplyToEach(H, qubits);

    // Apply Grover iterations.
    for (iteration in 1 .. numIterations) {
        MarkingOracle(qubits);
        DiffusionOperator(qubits);
    }
}
```
### Run Grover Algorithm - Entry Point

The `RunGroverAlgorithm` operation is the entry point for the quantum algorithm. It allocates qubits, runs the Grover algorithm, measures the result, and interacts with Azure Blob Storage.

```qsharp
operation RunGroverAlgorithm() : Unit {
    // Define the number of qubits needed based on the database size.
    mutable numQubits = 4; // Adjust this based on your specific needs.

    // Allocate qubits.
    using (qubits = Qubit[numQubits]) {
        // Run Grover's algorithm with a specified number of iterations.
        let numIterations = 2; // Adjust this based on your specific needs.
        let markedItem = 1; // Adjust this based on how you encode your marked item.

        GroverAlgorithm(numIterations, markedItem, qubits);
```
## Automated Deployement Pipeline - Azure

` Audomated Deplyoment Pipeline.yaml `

The Azure Quantum Workspace creation and QDK Application Setup steps are combined into separate script tasks.
The Storage Account Key is obtained and exported as an environment variable.
The steps for deploying the Q# application to Azure Quantum, retrieving the quantum job result, and downloading blob content are included.

## SQL Automation Script
`Automated Deployement.ps1`

The SQL automation script automates the deployment of the Q# application and the execution of the Quantum Grover algorithm on Azure Quantum. Ensure you have the Azure CLI installed and authenticated before running the script.

### Script Overview

#### Azure Quantum Workspace Creation:
The script creates an Azure Quantum Workspace if it doesn't exist.

#### Quantum Development Kit (QDK) Application Setup:
It sets up the Quantum Development Kit application, copying necessary files and configuring the QDK for Azure Quantum.

#### Azure Blob Storage Integration: 
The script sets up the connection string for Azure Blob Storage, specifies the container and blob names, and deploys the Q# application to Azure Quantum.

#### Job Submission and Result Retrieval: 
It submits the quantum job to Azure Quantum and retrieves the result.

#### Blob Content Processing: 
The script downloads the blob content based on the quantum algorithm result and performs processing.


### Usage
Follow the steps outlined in the prerequisites section and run the SQL script to automate the deployment and execution of the Quantum Grover algorithm on Azure Quantum.

