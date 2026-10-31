# Blockchain
import hashlib
import time

# Function to generate hash
def generate_hash(data):
    return hashlib.sha256(str(data).encode()).hexdigest()


# Block class
class Block:
    def __init__(self, data, previous_hash):
        self.data = data
        self.timestamp = time.time()
        self.previous_hash = previous_hash
        self.hash = generate_hash(
            str(data) + str(self.timestamp) + previous_hash
        )


# Blockchain class
class Blockchain:
    def __init__(self):
        self.chain = []
        self.chain.append(Block("Genesis Block", "0"))

    # Add new block
    def add_block(self, data):
        previous = self.chain[-1]
        new_block = Block(data, previous.hash)
        self.chain.append(new_block)

    # Display blockchain
    def display(self):
        for block in self.chain:
            print("\nData:", block.data)
            print("Timestamp:", block.timestamp)
            print("Previous Hash:", block.previous_hash)
            print("Block Hash:", block.hash)

    # Verify blockchain
    def verify(self):
        for i in range(1, len(self.chain)):
            current = self.chain[i]
            previous = self.chain[i-1]

            if current.previous_hash != previous.hash:
                return False

            new_hash = generate_hash(
                str(current.data) +
                str(current.timestamp) +
                current.previous_hash
            )

            if current.hash != new_hash:
                return False

        return True


# Main program
bc = Blockchain()

bc.add_block("Transaction 1")
bc.add_block("Transaction 2")
bc.add_block("Transaction 3")

bc.display()

print("\nBlockchain Valid:", bc.verify())
