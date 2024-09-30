

# codewars

## All Balanced Parentheses

https://www.codewars.com/kata/5426d7a2c2c7784365000783

```
import java.util.ArrayList;
import java.util.List;

public class BalancedParens {
    static List<String> res;
    static int sn;
    public static List <String> balancedParens (int n) {
        // your code here
        res = new ArrayList<>();
        sn = n;
        generate("",0,0);
        return res;
    }

    public static void generate(String parens, int opens, int closes) {
        if (opens == sn && closes == sn) {
            res.add(parens);
            return ;
        } else if (opens == sn) {
            String tmp = parens;
            for (int i = 0; i < sn-closes; i++) {
                tmp = tmp + ")";
            }
            res.add(tmp);
            return;
        }
        if (opens > closes) {
            generate(parens+")", opens, closes+1);
        }
        generate(parens + "(", opens + 1, closes);
    }
}
```

## Twice linear

https://www.codewars.com/kata/5672682212c8ecf83e000050

```
import java.util.ArrayList;
import java.util.List;

class DoubleLinear {

    public static int dblLinear (int n) {
        List<Integer> u = new ArrayList<>();
        u.add(1);
        int i=0,j=0;
        int y,z;
        while (u.size() <= n) {
            y = 2 * u.get(i) + 1;
            z = 3 * u.get(j) + 1;
            if (y < z) {
                u.add(y);
                i ++;
            } else if (y > z) {
                u.add(z);
                j++;
            } else {
                u.add(y);
                i++;
                j++;
            }
        }
        return u.get(u.size() - 1);
    }
}
```

## Roman Numerals Helper

https://www.codewars.com/kata/51b66044bce5799a7f000003

```
public class RomanNumerals {
    private static final String[] ROMAN_NUMBERS = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
    private static final int[] ARABIC_NUMBERS = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};

    public static String toRoman(int n) {
        int remainingValue = n;
        StringBuilder result = new StringBuilder();

        for (int i = 0; i < ARABIC_NUMBERS.length; i++) {
            while (remainingValue >= ARABIC_NUMBERS[i]) {
                remainingValue -= ARABIC_NUMBERS[i];
                result.append(ROMAN_NUMBERS[i]);
            }
        }

        return result.toString();
    }

    public static int fromRoman(String romanNumeral) {
        String remainingValue = romanNumeral;
        int result = 0;

        for(int i = 0; i<ROMAN_NUMBERS.length; i++) {
            while(remainingValue.startsWith(ROMAN_NUMBERS[i])) {
                remainingValue = remainingValue.substring(ROMAN_NUMBERS[i].length(), remainingValue.length());
                result += ARABIC_NUMBERS[i];
            }
        }
        return result;
    }
}
```

## Sums of Perfect Squares

https://www.codewars.com/kata/5a3af5b1ee1aaeabfe000084

```
public class SumOfSquares {

    public static int nSquaresFor(int n) {
        // Your code here!
        double j = Math.sqrt(Double.valueOf(n));

        if (j == Math.floor(j) && j * j == n) {
            return 1;
        }
        for (int i = 1; i * i <= n; i++) {
            double j2 = Math.sqrt(Double.valueOf(n - i * i));
            if (j2 == Math.floor(j2) && j2 * j2 == (n - i * i)) {
                return 2;
            }
        }
        while (n % 4 == 0) {
            n /= 4;
        }
        if (n % 8 == 7) {
            return 4;
        }
        return 3;

    }
}
```

## Human readable duration format

https://www.codewars.com/kata/human-readable-duration-format

```
public class TimeFormatter {
    
    public static String formatDuration(int seconds) {
        String[] units = {"year", "day", "hour", "minute", "second"};
        int[] secsPerUnit = {31536000, 86400, 3600, 60, 1};
        int[] durations = new int[units.length];
        StringBuilder ret = new StringBuilder();

        if (seconds == 0) {
            return "now";
        }

        // Calculate the duration for each time unit
        for (int i = 0; i < units.length; i++) {
            if (seconds >= secsPerUnit[i]) {
                durations[i] = seconds / secsPerUnit[i];
                seconds %= secsPerUnit[i];
            }
        }

        // Build the output string
        for (int j = 0; j < durations.length; j++) {
            if (durations[j] > 0) {
                if (ret.length() > 0) {
                    ret.append(", ");
                }
                String unit = durations[j] == 1 ? units[j] : units[j] + "s";
                ret.append(durations[j]).append(" ").append(unit);
            }
        }

        // Format the output for the last item
        String[] parts = ret.toString().split(", ");
        if (parts.length > 1) {
            ret = new StringBuilder(String.join(", ", java.util.Arrays.copyOf(parts, parts.length - 1)));
            ret.append(" and ").append(parts[parts.length - 1]);
        }

        return ret.toString();
    }
}
```

## Path Finder #2: shortest path

https://www.codewars.com/kata/57658bfa28ed87ecfa00058a

```
import java.util.LinkedList;
import java.util.Queue;

public class Finder {

    private static final char WALL = 'W';
    private static final int[][] DIRECTIONS = {
        {0, 1},  // right
        {1, 0},  // down
        {0, -1}, // left
        {-1, 0}  // up
    };

    public static int pathFinder(String maze) {
        String[] rows = maze.split("\n");
        int N = rows.length;

        // Check if starting point is the same as destination
        if (N == 1 && rows[0].charAt(0) == '.') {
            return 0; // already at the destination
        }

        int[][] distances = new int[N][N];
        
        // Initialize distances with -1
        for (int[] row : distances) {
            java.util.Arrays.fill(row, -1);
        }
        distances[0][0] = 0; // Starting point
        
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[]{0, 0}); // Start from (0, 0)

        while (!queue.isEmpty()) {
            int[] current = queue.poll();
            int r = current[0];
            int c = current[1];

            for (int[] dir : DIRECTIONS) {
                int newR = r + dir[0];
                int newC = c + dir[1];

                // Check bounds and conditions
                if (newR >= 0 && newR < N && newC >= 0 && newC < N 
                    && distances[newR][newC] == -1 
                    && rows[newR].charAt(newC) != WALL) {

                    distances[newR][newC] = distances[r][c] + 1;

                    // Check if we reached the destination
                    if (newR == N - 1 && newC == N - 1) {
                        return distances[newR][newC];
                    }

                    queue.offer(new int[]{newR, newC});
                }
            }
        }

        // If we couldn't reach the destination, return -1
        return -1; // Return -1 for unreachable
    }
}
```

