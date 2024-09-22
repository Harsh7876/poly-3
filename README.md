# Deploying a Verifier Contract on a Testnet


This project is part of the Metacrafters Poly Proof Advanced Course, Module 3. 
&nbsp;

In this project, we have to build a circuit using `circom`, compile a `Verifier Contract` and deploy it to the `sepolia Testnet`

&nbsp;

### Circuit Diagram
&nbsp;
![Circuit Diagram](https://authoring.metacrafters.io/assets/cms/Assessment_b05f6ed658.png?updated_at=2023-02-24T00:00:37.278Z)

&nbsp;

### Circuit Code

```
pragma circom 2.0.0;

/*This circuit template checks that c is the multiplication of a and b.*/  

template harsh79node () 
{  

    //signal inputs
    signal input a;  
    signal input b; 

    //signals from gates
    signal x;  
    signal y; 

    //final signal output
    signal output Q; 
    

    //component gate used to create custom circuit
     component andGate = AND();
     component notGate = NOT();
     component orGate = OR();

    //circuit logic
    andGate.A <== a;
    andGate.B <== b;
    x <== andGate.out;
  
    notGate.A <== b;
    y <== notGate.out;

    orGate.A <==x;
    orGate.B <==y;
    Q <== orGate.out; 
    
}

template AND() {
  signal input A;
  signal input B;
  signal output out;

  out <== A * B;
}

template NOT() {
  signal input A;
  signal output out;

  out <== 1 + A - 2 * A;
}

template OR() {
  signal input A;
  signal input B;
  signal output out;

  out <== A + B - A * B;
}

component main = harsh79node ();
```

&nbsp;

The circuit takes two input signals `A` & `B`. It contains each of the following gates `AND`, `NOT` and `OR`.

- `AND` gate takes `A` and `B` as input and gives signal `X` as output.
- `NOT` gate takes `B` as input and gives signal `Y` as output.
- `OR` gate takes `X` and `Y` as input and gives signal `Q` as output.

The circuit has been verified for the following input `A: 0` and `B: 1`

&nbsp;

### Project Setup

- Clone the repository
- Install the dependencies
    ```
    npm i
    ```
- Create a .env file to store wallet private key
    ```
    WALLET_PRIVATE_KEY= #YOUR_PRIVATE_KEY
    ```

### Circuit Compilation and Deployment

- Compile the circuit
    ```
    npx hardhat circom
    ```
- Deploy the verifier Contract
    ```
    npx hardhat run scripts/deploy.ts --network sepolia
    ```
    &nbsp;
The script will compile the contract and deploy it onto the Mumbai Testnet. The address of the contract will be printed on the console. 
