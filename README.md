# Cyclic Barrier
CyclicBarrier class is a synchronization mechanism that can synchronize threads progressing through some algorithm. In other words, **it is a barrier that all threads must wait at, until all threads reach it, before any of the threads can continue.**

```java
import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

public class MultiplayerGame {

    private static class Player extends Thread {
        private CyclicBarrier barrier;
        private int playerId;
        public Player(CyclicBarrier barrier, int playerId) {
            this.barrier = barrier;
            this.playerId = playerId;
        }

        @Override
        public void run() {
            try {
                System.out.println(Thread.currentThread().getName()+" , Player " + playerId + " setup is being prepared...");
                Thread.sleep((long) (Math.random() * 8000));  // Simulate preparation time

                System.out.println(Thread.currentThread().getName()+" , Player " + playerId + " is ready.");
                // Wait at the barrier for all players to be ready
                barrier.await();

                // This part will be executed once all players are ready
                System.out.println(Thread.currentThread().getName()+" , Player " + playerId + " starts the game!");
            } catch (InterruptedException | BrokenBarrierException e) {
                System.out.println(Thread.currentThread().getName()+" , Player " + playerId + " was interrupted.");
            }
        }
    }

    public static void main(String[] args) {
        int playerCount = 5;
        // Create a CyclicBarrier for 'playerCount' players with a barrier action
        CyclicBarrier barrier = new CyclicBarrier(playerCount, () -> {
            System.out.println("All players are ready. Let's start the game!");
        });
        // Create and start player threads
        for (int i = 1; i <= playerCount; i++) {
            new Player(barrier, i).start();
        }
    }
}
```

**Execution Output :**
```
Thread-3 , Player 4 setup is being prepared...
Thread-2 , Player 3 setup is being prepared...
Thread-4 , Player 5 setup is being prepared...
Thread-1 , Player 2 setup is being prepared...
Thread-0 , Player 1 setup is being prepared...
Thread-3 , Player 4 is ready.
Thread-2 , Player 3 is ready.
Thread-0 , Player 1 is ready.
Thread-4 , Player 5 is ready.
Thread-1 , Player 2 is ready.
All players are ready. Let's start the game!
Thread-3 , Player 4 starts the game!
Thread-0 , Player 1 starts the game!
Thread-2 , Player 3 starts the game!
Thread-4 , Player 5 starts the game!
Thread-1 , Player 2 starts the game!
```
