# SSCS_Synthoshpere

# 128/256-bit key AES (Advanced Encryption Standard) CIPHER DECIPHER Processor

AES is a widely used symmetric encryption algorithm that provides secure data encryption and decryption. It was established as a standard by the National Institute of Standards and Technology (NIST) in 2001. AES operates on blocks of data and supports key lengths of 128, 192, and 256 bits.

A 256-bit key AES refers to the variant of AES where a 256-bit key is used for both encryption and decryption. The algorithm operates on blocks of 128 bits at a time. Here's a high-level overview of how the AES encryption process works with a 256-bit key:

## Key Expansion: 
The 256-bit key is expanded into a key schedule that contains a series of round keys. These round keys are used in the successive rounds of encryption.

## Initial Round: 
AddRoundKey: The first round key is combined with the input data using a bitwise XOR operation.

## Main Rounds (Multiple Rounds):

### SubBytes: 
Each byte of the block is replaced with a corresponding byte from the S-box, a predefined substitution table.
### ShiftRows: 
The rows of the block are shifted to the left by different offsets.
### MixColumns: 
The columns of the block are mixed to provide diffusion.
### AddRoundKey: 
The round key for the current round is combined with the data using XOR.
### Final Round:

#### SubBytes
#### ShiftRows
#### AddRoundKey
#### The number of main rounds depends on the key length and block size:

For a 256-bit key, there are 14 rounds.
After all the rounds are completed, the encrypted data is obtained.

#### Decryption is essentially the reverse process:

## Key Expansion: The 256-bit key is expanded to generate round keys.

## Initial Round:
AddRoundKey: The last round key is combined with the encrypted data.

## Main Rounds (Multiple Rounds):

Inverse operations of SubBytes, ShiftRows, and MixColumns are applied in reverse order.
AddRoundKey: The round key for the current round is combined with the data.

## Final Round:

### Inverse SubBytes
### Inverse ShiftRows
### AddRoundKey
At the end of this process, the original plaintext is obtained.

The strength of AES lies in its key length. A 256-bit key provides a high level of security against brute-force attacks. The algorithm's design and its resistance to various cryptographic attacks make it a widely trusted choice for securing sensitive data.

This implementation supports 128 and 256 bit keys. The implementation is iterative and process one 128 block at a time. Blocks are processed on a word level with 4 S-boxes in the data path. The S-boxes for encryption are shared with the key expansion and the core can thus not do key update in parallel with block processing.

The encipher and decipher block processing datapaths are separated and basically self contained given access to a set of round keys and a block. This makes it possible to hard wire the core to only encipher or decipher operation. This allows the synthesis/build tools to optimize away the other functionality which will reduce the size to about 50%. This has been tested to verify that decryption is removed and the core still works.

## Algorithm :

![aesfull](https://github.com/pratheekgg/SSCS_Synthoshpere/assets/121636887/eb92a760-667c-4923-a462-242fb1261320)


# DOCUMENTATION :

Compilation of the RTL was made using opensource software iverilog for the design.v and test.v files in my testfiles folder.

![Screenshot from 2023-08-26 06-36-03](https://github.com/pratheekgg/SSCS_Synthoshpere/assets/121636887/8f43a6b4-b4fd-48fa-a277-d01bfaa93266)

### Result of Compilation :

![Screenshot from 2023-08-26 06-34-10](https://github.com/pratheekgg/SSCS_Synthoshpere/assets/121636887/da7969aa-a583-450f-bdc8-b40928e7d2d4)

Then another opensource software gtkwave was used to generate the waveform of the simulation from the generated dump.vcd file.

### Simulation results :

![Screenshot from 2023-08-26 06-09-23](https://github.com/pratheekgg/SSCS_Synthoshpere/assets/121636887/9daf277b-fbc8-4e57-a17b-456c32ec1768)

The outputs are hexadecimal texts that were encrypted by the aes_encipher_block at the transmitter and decrypted by the aes_decipher_block at the reciever.

The top module of this processor rtl design 'aes' consists of submodules aes_core, aes_enciphering_block, aes_deciphering_block, aes_inv_sbox, aes_sbox and memory unit.
The command used to set 'aes' as top module :

```
synth -top aes
```
<img width="748" alt="1" src="https://github.com/pratheekgg/SSCS_Synthoshpere/assets/121636887/972d2691-92b0-40c1-87cf-337d85f4ef48">

### Result of top module optimization : 

![Screenshot from 2023-08-26 06-39-22](https://github.com/pratheekgg/SSCS_Synthoshpere/assets/121636887/9d349d99-c1dd-4a47-b3e5-bce370092c9d)

![Screenshot from 2023-08-26 06-39-35](https://github.com/pratheekgg/SSCS_Synthoshpere/assets/121636887/76eebbdc-1db4-4770-aaee-a182e7ff853e)

![Screenshot from 2023-08-26 06-39-42](https://github.com/pratheekgg/SSCS_Synthoshpere/assets/121636887/39b0a093-eb83-40b5-99b6-19a41ebd90bc)

### Result of Flipflop re-mapping :

![Screenshot from 2023-08-26 06-48-22](https://github.com/pratheekgg/SSCS_Synthoshpere/assets/121636887/48ca6386-2b51-46f1-885f-8007944bc246)


Command : 
```
abc -liberty ../lib/sky130_fd_sc_hd_tt_025c_1v80.lib
```

![Screenshot from 2023-08-26 06-55-17](https://github.com/pratheekgg/SSCS_Synthoshpere/assets/121636887/e4db9e5a-e0af-41e7-a2a8-0a197bd4a25b)









