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
// Multiple classes for text analysis
abstract class KeywordAnalyzer implements TextAnalyzer {
    
    protected abstract Label getLabel();
    
    protected abstract String[] getKeywords();
    
    public Label processText(String text) {
        String[] keywords = getKeywords();
        for (String keyword : keywords) {
            if (text.contains(keyword)) {
                return getLabel();
            }
        }
        return Label.OK;
    }
    
}

class SpamAnalyzer extends KeywordAnalyzer {
   
    private String[] keywords;
    
    public SpamAnalyzer(String[] keywords) {
        this.keywords = keywords;
    }
    
    @Override
    public Label getLabel() {
        return Label.SPAM;
    }
    
    @Override
    public String[] getKeywords() {
        return this.keywords;
    }
    
}

class NegativeTextAnalyzer extends KeywordAnalyzer {
    
    @Override
    public Label getLabel() {
        return Label.NEGATIVE_TEXT;
    }
    
    @Override
    public String[] getKeywords() {
        return new String[]{":(", "=(", ":|"};
    }
    
}

class TooLongTextAnalyzer implements TextAnalyzer {
    
    private int maxLength;
    
    public TooLongTextAnalyzer(int maxLength) {
        this.maxLength = maxLength;
    }
    
    public Label processText(String text) {
        if (text.length() > maxLength) {
            return Label.TOO_LONG;
        }
        return Label.OK;
    }
}

public Label checkLabels(TextAnalyzer[] analyzers, String text) {
    for (TextAnalyzer analyzer : analyzers) {
        Label result = analyzer.processText(text);
        if (result == Label.OK) {
            continue;
        }
        return result;
    }
    return Label.OK;
}
```

## Exceptions

```java
// Square root calculation with input control
public static double sqrt(double x) {
    if (x >= 0) {
        return Math.sqrt(x);
    } 
    else {
        throw new java.lang.IllegalArgumentException("Expected non-negative number, got " + x);
    }
}
```

```java
// Method for geting class name and method
public static String getCallerClassAndMethodName() {
    StackTraceElement[] ste = new Throwable().getStackTrace();
    if (ste.length >= 3) {
        return ste[2].getClassName() + "#" + ste[2].getMethodName();
    }
    return null;
}
```

```java
// Robot control with exception handling
public static void moveRobot(RobotConnectionManager robotConnectionManager, int toX, int toY) {
    final int RETRIES = 3;
    for (int i = 1; i <= RETRIES; i++) {
        try (RobotConnection connection = robotConnectionManager.getConnection()) {
            connection.moveRobotTo(toX, toY);
            i = Integer.MAX_VALUE - 1;
        }
        catch (RobotConnectionException re) {
            if (i == RETRIES) {
                throw new RobotConnectionException("Max retries reached.", re);
            }
        }
    }
}
```

## Logging

```java
// Logging configuration
private static void configureLogging() {
    Logger loggerA = Logger.getLogger("org.stepic.java.logging.ClassA");
    loggerA.setLevel(Level.ALL);
    Logger loggerB = Logger.getLogger("org.stepic.java.logging.ClassB");
    loggerB.setLevel(Level.WARNING);
    Logger loggerC = Logger.getLogger("org.stepic.java");
    loggerC.setLevel(Level.ALL);
    Formatter formatter = new XMLFormatter();
    Handler handler = new ConsoleHandler();
    handler.setFormatter(formatter);
    handler.setLevel(Level.ALL);
    loggerC.addHandler(handler);
    loggerC.setUseParentHandlers(false);
} 
```

```java
// Mail system model
public static class UntrustworthyMailWorker implements MailService {
    
    private final MailService[] recipients;
    private final RealMailService realMailService;
    
    public UntrustworthyMailWorker(MailService[] recipients) {
        this.recipients = recipients;
        realMailService = new RealMailService();
    }
    
    public RealMailService getRealMailService() {
        return realMailService;
    }
    
    @Override
    public Sendable processMail(Sendable mail) {
        for (MailService recipient : recipients) {
            mail = recipient.processMail(mail);
        }
        return getRealMailService().processMail(mail);
    }
    
}

public static class Spy implements MailService {
    
    private final Logger logger;
    
    public Spy(Logger logger) {
        this.logger = logger;
    }
    
    @Override
    public Sendable processMail(Sendable mail) {
        if (mail instanceof MailMessage) {
            if (mail.getFrom().equals(AUSTIN_POWERS) ||
                mail.getTo().equals(AUSTIN_POWERS)) {
                logger.log(Level.WARNING, "Detected target mail correspondence: from {0} to {1} \"{2}\"",
                              new Object[] {mail.getFrom(), mail.getTo(), ((MailMessage)mail).getMessage()});
            } 
            else {
                logger.log(Level.INFO, "Usual correspondence: from {0} to {1}",
                           new Object[] {mail.getFrom(), mail.getTo()});
            }      
        }
        return mail;
    }
    
}

public static class Thief implements MailService {
    
    private final int minValue;
    private int stolenValue;
    
    public Thief(int minValue) {
        this.minValue = minValue;
        stolenValue = 0;
    }
    
    public int getStolenValue() {
        return stolenValue;
    }
    
    @Override
    public Sendable processMail(Sendable mail) {
        if (mail instanceof MailPackage  && ((MailPackage)mail).getContent().getPrice() >= minValue) {
            stolenValue += ((MailPackage)mail).getContent().getPrice();
            return (new MailPackage(mail.getFrom(), mail.getTo(),
                                    new Package("stones instead of " + 
                                                ((MailPackage)mail).getContent().getContent(), 0)));
        } 
        else {
            return mail;
        }
    }
    
}

public static class Inspector implements MailService {
    
    @Override
    public Sendable processMail(Sendable mail) {
        if (mail instanceof MailPackage) {
            String content = ((MailPackage)mail).getContent().getContent();
            if (content.contains(WEAPONS) || content.contains(BANNED_SUBSTANCE)) {
                throw new IllegalPackageException();
            }
            if (content.contains("stones")) {
                throw new StolenPackageException();
            }
        }
        return mail;
    }
    
}

public static class IllegalPackageException extends  RuntimeException {}

public static class StolenPackageException extends  RuntimeException {} 
```

## File system IO

```java
// Read input stream and  calculate checksum
public static int checkSumOfStream(InputStream inputStream) throws IOException {
    int cs = 0;
    int data;
    while ((data = inputStream.read()) >= 0) {
        cs = Integer.rotateLeft(cs, 1) ^ data;
    }
    return cs;
}
```

```java
// Windows to unix linefeed (new line) converter
import java.io.IOException;

public class Main {
    
    public static void main(String[] args) throws IOException {
        int dataCur;
        int dataPrew = -1;
        while ((dataCur = System.in.read()) >= 0) {
            if (dataCur == 10 && dataPrew == 13) {
                dataPrew = -1;
                System.out.write(dataCur);
            }
            else {
                if (dataPrew >= 0) {
                    System.out.write(dataPrew);
                    dataPrew = dataCur;
                }
                dataPrew = dataCur;
            }
        }
        if (dataCur < 0 && dataPrew >= 0) {
            System.out.write(dataPrew);
        }
        System.out.flush();
    }
    
}
```

```java
// Stream to String converter
public static String readAsString(InputStream inputStream, Charset charset) throws IOException {
    try (InputStreamReader reader = new InputStreamReader(inputStream, charset)) {
        StringBuilder sb = new StringBuilder();
        int c;
        while ( (c = reader.read()) >=0) {
            sb.append((char)c);
        }
        return sb.toString();
    }
}
```

```java
// Sum of numbers from stream
import java.util.Scanner;
import java.util.Locale;

public class Main {
    
    public static void main(String[] args) {
        Double acc = 0.0d;
        try (Scanner reader = new Scanner(System.in);) {
            reader.useLocale(Locale.ENGLISH);
            while (reader.hasNext()) {
                if (reader.hasNextDouble()){
                    acc += reader.nextDouble();
                } else {
                    reader.next();
                }
            }
        }
        System.out.printf("%.6f", acc);
        System.out.flush();
    }
    
}
```

```java
// Input stream to objects
public static Animal[] deserializeAnimalArray(byte[] data) {
    try (ObjectInputStream ois = new ObjectInputStream(new ByteArrayInputStream(data));) {
        int size = ois.readInt();
        Animal[] animals = new Animal[size];
        for (int i = 0; i < size; i++) {
            animals[i] = (Animal)ois.readObject();
        }
        return animals;
    } catch (Exception e) {
        throw new IllegalArgumentException();
    }
}
```

## Generics

```java
// Extension of Optional for pair of objects
class Pair<T,S> {
    private final T t;
    private final S s;
    

    
    private Pair(T t, S s) {
        this.t = t;
        this.s = s;
    }
    
    public static <T,S> Pair<T,S> of(T t, S s) {
        return new Pair<>(t, s);
    }
    
    public T getFirst() {
        return t;
    }
    
    public S getSecond() {
        return s;
    }
    
    public boolean equals(Object obj){
        if (this == obj) {
            return true;
        }
        
        if (!(obj instanceof Pair)) {
            return false;
        }
        
        Pair<?,?> other = (Pair<?,?>) obj;
        return Objects.equals(other.getFirst(), this.t) && Objects.equals(other.getSecond(), this.s);
    }
    
    public int hashCode() {
        return Objects.hashCode(t) ^ Objects.hashCode(s);
    }
}
```
## Collections

```java
// Symmetric difference of two sets
public static <T> Set<T> symmetricDifference(Set<? extends T> set1, Set<? extends T> set2) {
    Set<T> set = new LinkedHashSet<>();
    for (T element : set1) {
        if (!(set2.contains(element))) {
            set.add(element);
        }
    }
    for (T element : set2) {
        if (!(set1.contains(element))) {
            set.add(element);
        }
    }
    return set;
}
```

```java
// Reverse odd numbers
import java.util.LinkedList;
import java.util.Deque;
import java.util.Scanner;

public class Main { 
    public static void main(String[] args) {
        boolean even = true;
        LinkedList<Integer> l = new LinkedList<>();
        Scanner sc = new Scanner(System.in);
        while (sc.hasNextInt()) {
            Integer i = sc.nextInt();
            if (!even) {
                l.addFirst(i);
            }
            even = !even;
        }
        l.forEach((x)->System.out.printf("%s ",x));
    }
}
```
## Functional interfaces
```java
// Composition of functions
public static <T, U> Function<T, U> ternaryOperator(
        Predicate<? super T> condition,
        Function<? super T, ? extends U> ifTrue,
        Function<? super T, ? extends U> ifFalse) {

    return t -> condition.test(t) ? ifTrue.apply(t) : ifFalse.apply(t); 
}
```

## Stream API

```java
// Generate pseudo random stream
public static IntStream pseudoRandomStream(int seed) {
    return IntStream.iterate(seed, x-> x*x%10000/10); // your implementation here
}
```

```java
// Min and max element in stream by given comparator
public static <T> void findMinMax(
        Stream<? extends T> stream,
        Comparator<? super T> order,
        BiConsumer<? super T, ? super T> minMaxConsumer) {
    List<? extends T> l = stream.collect(Collectors.toList());
    Stream<? extends T> stream1 = l.stream();
    Stream<? extends T> stream2 = l.stream();
    Optional<? extends T> min = stream1.min(order);
    Optional<? extends T> max = stream2.max(order);
    minMaxConsumer.accept(min.isPresent()?min.get():null, max.isPresent()?max.get():null);
}
```

```java
// Set of most frequent words in stream
import java.util.stream.*;
import java.util.List;
import java.util.LinkedList;
import java.io.IOException;
import java.util.Scanner;
import java.util.Map.Entry;
import java.util.Comparator;
import java.util.AbstractMap.SimpleEntry;
public class Main {
    public static void main(String[] args)  throws IOException   {
       List<String> s = new LinkedList<>();
       Scanner sc = new Scanner(System.in).useDelimiter("[^А-Яа-я0-9A-Za-z]+"); 
       while (sc.hasNext()) {
          s.add(sc.next().toLowerCase());
       }
       sc.close();
       s.stream()
           .collect(Collectors.groupingBy(Object::toString)).entrySet()
           .stream()
           .sorted(Comparator.comparing(Entry::getKey))
           .sorted((Comparator.comparing(x->x.getValue().size()*-1)))
           .map(x->x.getKey())
           .limit(10)
           .forEach(System.out::println);
    }
}
```

```java
// Mail service model with mailbox
public interface Message<T> {
    public String getTo();
    public String getFrom();
    public T getContent();
}

public abstract static class MailUnit<T> implements Message<T> {
    private final String from;
    private final String to;
    private final T content;
    
    public  MailUnit(String from, String to, T content) {
        this.from = from;
        this.to = to;
        this.content = content;
    }
    
    public String getFrom() {
        return from;
    }
    public String getTo() {
        return to;
    }
    
    public T getContent() {
        return content;
    }
}

public static class MailMessage extends MailUnit<String> {
    public MailMessage(String from, String to, String content) {
        super(from, to, content);
    }
}

public static class Salary extends MailUnit<Integer> {
    public Salary(String from, String to, Integer salary) {
        super(from, to, salary);
    }
}

public static class MailService<T> implements Consumer<Message<T>> {
    
    private Map<String, List<T>> mailBox;
    
    public MailService() {
        mailBox = new HashMap<String, List<T>>() {
            @Override
            public List<T> get(Object key) {
                return super.getOrDefault(key, new LinkedList<T>());
            }
        };
    }
    
    public void accept(Message<T> t) {
        if (mailBox.containsKey(t.getTo())) {
            List<T> val = mailBox.get(t.getTo());
            val.add(t.getContent());
            mailBox.put(t.getTo(),val);
        }
        else {
            List<T> val = new LinkedList<T>();
            val.add(t.getContent());
            mailBox.put(t.getTo(),val);
        }
           
    }
    
    public Map<String, List<T>> getMailBox(){
        return mailBox;
    }
    
}
```