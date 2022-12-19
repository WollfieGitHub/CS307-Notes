# MESI Protocol
Same as [[MSI Protocol]] but with an **Exclusive** state indicating the line is the only line to hold a valid copy of the data

- On writing to a block in E, can proceed without broadcast
- When evicting a block in E, can proceed without writeback

