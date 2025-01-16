# Wallet Discovery Module and Testbench Documentation

## Overview
The Wallet Discovery module is designed to generate sequential private keys, derive their corresponding public keys and wallet addresses, and compare them against a user-defined vanity hash pattern. The module supports parallel processing through multiple pipelines to enhance performance.

The testbench verifies the functionality of the Wallet Discovery module, including clock-driven operations, reset behavior, and edge cases for pattern matching.

---

## Wallet Discovery Module

### Features
1. **Parallel Processing**:
   - The module uses multiple pipelines (`NUM_PIPELINES`) to generate keys and perform vanity hash matching simultaneously.
2. **Configurable Parameters**:
   - `NUM_PIPELINES`: Determines the number of parallel processing units.
   - `KEY_WIDTH`: Specifies the bit-width of the private key.
   - `HASH_WIDTH`: Specifies the bit-width of the hash output (compatible with SHA-256).
3. **Reset Support**:
   - Resets all internal states and restarts the key generation sequence.

### Inputs and Outputs
   Signal                  Width       Direction         Description                                              

  `clk`                   1-bit         Input        Clock signal driving the module.                        
  `reset`                1-bit          Input       Resets the module when asserted.                        
  `vanity_pattern`       256-bit        Input        User-defined hash pattern for vanity matching.          
  `match_found`           1-bit         Output      Indicates if a matching hash was found.                 
  `matching_private_key`  256-bit       Output      Outputs the private key corresponding to the match.     

### Functional Description
The module performs the following steps:
1. **Private Key Generation**:
   - Each pipeline generates sequential private keys, starting from a unique initial value.
2. **ECC Public Key Derivation**:
   - The private key is used to compute the corresponding public key (stubbed for simplicity).
3. **SHA-256 Hashing**:
   - The public key is hashed to obtain the wallet address.
4. **Vanity Hash Matching**:
   - The hash is compared with the user-defined pattern. If a match is found, the private key is recorded and output.

---

## Testbench

### Features
1. **Clock Signal Generation**:
   - A 10ns clock period is generated for module operation.
2. **Reset Testing**:
   - Asserts and deasserts the reset signal to verify module initialization.
3. **Vanity Pattern Testing**:
   - Tests the module with various patterns to validate functionality.
4. **Edge Cases**:
   - Simulates scenarios with low key ranges and pattern changes during operation.

### Testbench Components
 Signal                 Width         Direction     Description                                              

 `clk`                   1-bit         Input      Clock signal driving the testbench.                     
 `reset`                 1-bit         Input       Resets the DUT (Device Under Test).                     
 `vanity_pattern`        256-bit       Input       Pattern for testing vanity matching.                    
 `match_found`           1-bit         Output      Monitors if the DUT finds a match.                      
 `matching_private_key`  256-bit       Output      Outputs the matching private key from the DUT.          

### Test Scenarios
#### 1. **Basic Functionality**:
- **Objective**: Verify if the DUT correctly identifies a match for a given pattern.
- **Setup**:
  - Initialize with `reset = 1`.
  - Provide `vanity_pattern = 256'hCAFEBABECAFEBABECAFEBABECAFEBABE`.
- **Expected Result**: `match_found` is asserted, and `matching_private_key` contains the corresponding private key.

#### 2. **Pattern Change**:
- **Objective**: Test the DUT’s behavior when the pattern changes mid-simulation.
- **Setup**:
  - Provide an initial pattern `256'hCAFEBABECAFEBABECAFEBABECAFEBABE`.
  - Change to `256'hDEADBEEFDEADBEEFDEADBEEFDEADBEEF` after 200ns.
- **Expected Result**: The DUT continues processing with the updated pattern.

#### 3. **Reset Behavior**:
- **Objective**: Verify module reset functionality.
- **Setup**:
  - Assert `reset = 1` for 20ns.
  - Deassert `reset` and resume operation.
- **Expected Result**: All internal states are reinitialized.

#### 4. **Edge Case: Low Key Range**:
- **Objective**: Simulate low key values to test robustness.
- **Setup**:
  - Set `vanity_pattern = 256'h00000000000000000000000000000000`.
- **Expected Result**: The DUT processes the low key range without errors.

---

## Simulation Outputs

1. **Waveform Analysis**:
   - Use `wallet_discovery_tb.vcd` for waveform visualization.
   - Observe clock, reset, and output signals.
2. **Simulation Logs**:
   - Check `$display` messages for match status and corresponding private keys.

---

## Future Enhancements
1. **ECC and SHA-256 Implementation**:
   - Replace stubs with optimized IP cores for hardware implementation.
2. **Resource Optimization**:
   - Analyze FPGA utilization and optimize the number of pipelines.
3. **Additional Test Cases**:
   - Test with randomized patterns and extended key ranges.

---

## Conclusion
This document provides a comprehensive overview of the Wallet Discovery module and its testbench. The system demonstrates sequential key generation, hash computation, and vanity matching with support for parallelism and reset functionality. The testbench validates the module under various scenarios, ensuring robust performance for potential FPGA deployment.

