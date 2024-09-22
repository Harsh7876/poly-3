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


template MyAwesomeCircuit () {  

   // input signal  
   signal input a;  
   signal input b;

   // gate signal
   signal x;
   signal y;
   
   // output signal
   signal output q;

   // components
   component andGate = AND();
   component orGate = OR();
   component notGate = NOT();

   // circuit  

   andGate.a <== a;
   andGate.b <== b;
   x <== andGate.out;

   notGate.in <== b;
   y <== notGate.out;

   orGate.a <== x;
   orGate.b <== y;
   q <== orGate.out;

}

template AND() {
    signal input a;
    signal input b;
    signal output out;

    out <== a*b;
}

template OR() {
    signal input a;
    signal input b;
    signal output out;

    out <== a + b - a*b;
}

template NOT() {
    signal input in;
    signal output out;

    out <== 1 + in - 2*in;
}

component main = MyAwesomeCircuit();
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
