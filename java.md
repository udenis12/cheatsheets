# Java core
Tasks from sptepik [course](https://stepik.org/course/187/syllabus).

## Base sintax


```java
// Return true for strictly two arguments is true
public static boolean booleanExpression(boolean a, boolean b, boolean c, boolean d) {
    return (a ^ b) & (c ^ d) || (a ^ c) & (b ^ d);
}
```


```java
// Count leap years
public static int leapYearCount(int year) {
    return year / 4 - year / 100 + year / 400;
}
```


```java
// Check difference with specified precision
public static boolean doubleExpression(double a, double b, double c) {
    return Math.abs(c - a - b) <= 0.0001;
}
```


```java
// Flip bit by index in integer
public static int flipBit(int value, int bitIndex) {
    return value ^ (1 << (bitIndex - 1));
}
```

```java
// Return char in specified range from given char
public static char charExpression(int a) {
    return (char)('\\' + a);
}
```

```java
// Check if given number is power of two
public static boolean isPowerOfTwo(int value) {
    return ((Math.abs(value) - 1) ^ Math.abs(value)) == Math.abs(value) - 1 + Math.abs(value) && value != 0;
}
```

```java
// Check if given string is palindrome
public static boolean isPalindrome(String text) {
    text = text.replaceAll("[^a-zA-Z0-9]", "");
    return text.equalsIgnoreCase(new StringBuffer(text).reverse().toString());
}
```

```java
// Factorial via BigInteger
public static BigInteger factorial(int value) {
    BigInteger result = BigInteger.ONE;
    for (int i = 1; i <= value; i++) {
        result = result.multiply(BigInteger.valueOf(i));
    }
    return result;
}
```

```java
// Merge arrays
public static int[] mergeArrays(int[] a1, int[] a2) {
    int[] r = new int[a1.length + a2.length];
    int ia1 = 0;
    int ia2 = 0;
    for (int i = 0; i < a1.length + a2.length; i++){
        if (ia2 >= a2.length || ia1 < a1.length && a1[ia1] <= a2[ia2]) {
            r[i] = a1[ia1];
            ia1++;
        }
        else {
            r[i] = a2[ia2];
            ia2++;
        }
    }
    return r;
}
```

```java
// Parse text and layout
private String printTextPerRole(String[] roles, String[] textLines) {
    StringBuilder[] rLines = new StringBuilder[roles.length];
    for (int i = 0; i < roles.length; i++) {
        rLines[i] = new StringBuilder(roles[i]).append(":\n");
    }
    int index = 0;
    for (String line : textLines) {
        index++;
        for (int i = 0; i < roles.length; i++) {
            if (line.startsWith(roles[i] + ": ")) {
                rLines[i].append(index).append(") ").append(line.substring(roles[i].length() + 2, line.length())).append("\n");
                break;
            }
        }
    }
    for (int i = 1; i < roles.length; i++) {
        rLines[0].append("\n").append(rLines[i]);
    }
    rLines[0].append("\n");
    return rLines[0].toString();
}
```

## Objects and classes

```java
// Moving robot to specified position
public static void moveRobot(Robot robot, int toX, int toY) {
    int deltaX = toX - robot.getX();
    int deltaY = toY - robot.getY();
    int rotateX = 0;
    int rotateY = 0;
    while (deltaX != 0 || deltaY != 0) {
        Direction currentDir = robot.getDirection();
        if (deltaX > 0 && currentDir == Direction.RIGHT || 
            deltaX < 0 && currentDir == Direction.LEFT) {
            for (int i = 0; i < Math.abs(deltaX); i++) {
                robot.stepForward();
            }
            deltaX = toX - robot.getX();
            continue;
        }
        if (deltaY > 0 && currentDir == Direction.UP || 
            deltaY < 0 && currentDir == Direction.DOWN) {
            for (int i = 0; i < Math.abs(deltaY); i++) {
                robot.stepForward();
            }
            deltaY = toY - robot.getY(); 
            continue;
        }
        
        rotateX = 0;
        rotateY = 0;
        if (currentDir == Direction.UP) {
            if (deltaX > 0) {
                rotateX = 1;
            }
            if (deltaX < 0) {
                rotateX = -1;
            }
            if (deltaY > 0) {
                continue;
            }
            if (deltaY < 0) {
                rotateY = 2;
            }
        }
        if (currentDir == Direction.DOWN) {
            if (deltaX < 0) {
                rotateX = 1;
            }
            if (deltaX > 0) {
                rotateX = -1;
            }
            if (deltaY < 0) {
                continue;
            }
            if (deltaY > 0) {
                rotateY = 2;
            }
        }
        if (currentDir == Direction.LEFT) {
            if (deltaY > 0) {
                rotateY = 1;
            }
            if (deltaY < 0) {
                rotateY = -1;
            }
            if (deltaX < 0) {
                continue;
            }
            if (deltaX > 0) {
                rotateX = 2;
            }
        }    
        if (currentDir == Direction.RIGHT) {
            if (deltaY < 0) {
                rotateY = 1;
            }
            if (deltaY > 0) {
                rotateY = -1;
            }
            if (deltaX > 0) {
                continue;
            }
            if (deltaX < 0) {
                rotateX = 2;
            }
        }               
        if (rotateX == 1 || rotateY == 1) {
            robot.turnRight();
            rotateX = 0;
            rotateY = 0;
            continue;
        } 
        if (rotateX == -1 || rotateY == -1) {
            robot.turnLeft();
            rotateX = 0;
            rotateY = 0;
            continue;
        } 
        if (rotateX == 2 || rotateY == 2) {
            robot.turnRight();
            robot.turnRight();
            rotateX = 0;
            rotateY = 0;
            continue;
        } 
    }
}
```

```java
// New class with override equals and hashCode methods
public final class ComplexNumber {
    private final double re;
    private final double im;

    public ComplexNumber(double re, double im) {
        this.re = re;
        this.im = im;
    }

    public double getRe() {
        return re;
    }

    public double getIm() {
        return im;
    }
    
    @Override
    public boolean equals(Object object) {
        if (this == object) {
            return true;
        }
        if (object instanceof ComplexNumber) {
            return Double.compare(this.getRe(), ((ComplexNumber)object).getRe())==0 && Double.compare(this.getIm(), ((ComplexNumber)object).getIm())==0 ? true : false;
        }
        return false;
    }
    
    @Override
    public int hashCode() {
        return Double.valueOf(this.getRe()).hashCode() + ~Double.valueOf(this.getIm()).hashCode();
    }
}
```

```java
// Integral by Riemann sum (left rectangles)
public static double integrate(DoubleUnaryOperator f, double a, double b) {
    double step = b - a;
    double resultOld = 0.0 / 0.0;
    double resultNew = 0.0 / 0.0;
    double delta = Double.POSITIVE_INFINITY;
    do {
        resultOld = resultNew;
        step /= 10.0;
        resultNew = 0.0;
        for (double x = a; x < b; x += step) {
            resultNew += f.applyAsDouble(x) * step;   
        }  
        if (!Double.valueOf(resultOld).isNaN()) {
            delta = Math.abs(resultOld - resultNew);
        }
    } 
    while (delta > 1E-2);
    return resultNew;
}
```

```java
// One byte sequence by CharSequence implementation
public class AsciiCharSequence implements java.lang.CharSequence {
    
    private byte[] data;
    
    public AsciiCharSequence(byte[] data) {
        this.data = data;
    }
    
    public int length() {
        return this.data.length;
    }
    
    public char charAt(int index) {
        return (char)data[index];
    }
    
    public AsciiCharSequence subSequence(int start, int end) {
        byte[] data = new byte[end - start];
        System.arraycopy(this.data, start, data, 0, end - start);
        return new AsciiCharSequence(data);  
    }
    
    public String toString() {
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < this.length(); i++) {
            result.append((char)this.data[i]);
        }
        return result.toString();
    }
}
```

```java
// 
```

```java
// 
```

