# Cyclic Barrier

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
