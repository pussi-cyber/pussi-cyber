import hashlib
import time
import multiprocessing

class SimpleMiner:
    def __init__(self, prefix_zeros):
        self.prefix_zeros = prefix_zeros
        self.prefix_str = '0' * prefix_zeros

    def mine(self, block_number, transactions, previous_hash):
        nonce = 0
        print("Розпочинаємо майнінг...")
        start_time = time.time()

        while True:
            block_data = f"{block_number}{transactions}{previous_hash}{nonce}".encode()
            block_hash = hashlib.sha256(block_data).hexdigest()

            if block_hash.startswith(self.prefix_str):
                print(f"Майнінг завершено! Нонсе: {nonce} Хеш: {block_hash}")
                break

            nonce += 1

            # Додаткове навантаження на ЦП
            for _ in range(1000000):  # Інтенсивне обчислення
                _ = sum(i * i for i in range(1000))  # Збільшено обчислення

            if nonce % 10000 == 0:
                print(f"Оброблено {nonce} нонсов...")

        print(f"Час виконання: {time.time() - start_time} секунд")

def start_mining_process():
    block_number = 1
    transactions = "A->B:5; C->D:10"
    previous_hash = "0" * 64
    prefix_zeros = 4  # Кількість нулів на початку хешу

    miner = SimpleMiner(prefix_zeros)
    miner.mine(block_number, transactions, previous_hash)

if __name__ == "__main__":
    # Створюємо кілька процесів для підвищення навантаження
    num_processes = multiprocessing.cpu_count()
    processes = []

    for _ in range(num_processes):
        p = multiprocessing.Process(target=start_mining_process)
        processes.append(p)
        p.start()

    for p in processes:
        p.join()
 shhhhhhh dont copy this or die
