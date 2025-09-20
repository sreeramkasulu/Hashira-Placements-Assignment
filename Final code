import java.math.BigInteger;
import java.util.ArrayList;
import java.util.List;
import java.util.regex.Matcher;
import java.util.regex.Pattern;


public class Main {

   
    static class Point {
        BigInteger x;
        BigInteger y;

        Point(BigInteger x, BigInteger y) {
            this.x = x;
            this.y = y;
        }
    }

    
    public static BigInteger findSecret(String jsonInput) {
       
        Pattern kPattern = Pattern.compile("\"k\":\\s*(\\d+)");
        Matcher kMatcher = kPattern.matcher(jsonInput);
        if (!kMatcher.find()) {
            throw new IllegalArgumentException("Could not find 'k' in the JSON input.");
        }
        int k = Integer.parseInt(kMatcher.group(1));

        List<Point> points = new ArrayList<>();
        
        Pattern pointPattern = Pattern.compile("\"(\\d+)\":\\s*\\{\\s*\"base\":\\s*\"(\\d+)\",\\s*\"value\":\\s*\"([a-zA-Z0-9]+)\"");
        Matcher pointMatcher = pointPattern.matcher(jsonInput);

       
        while (pointMatcher.find() && points.size() < k) {
            BigInteger x = new BigInteger(pointMatcher.group(1));
            int base = Integer.parseInt(pointMatcher.group(2));
            String value = pointMatcher.group(3);
            
            BigInteger y = new BigInteger(value, base);
            
            points.add(new Point(x, y));
        }
        
        if (points.size() < k) {
             throw new IllegalStateException("Did not find enough points in the JSON. Required: " + k + ", Found: " + points.size());
        }

        return lagrangeInterpolateAtZero(points);
    }

    
    private static BigInteger lagrangeInterpolateAtZero(List<Point> points) {
        BigInteger secret = BigInteger.ZERO;
        int k = points.size();

        for (int i = 0; i < k; i++) {
            BigInteger numerator = BigInteger.ONE;
            BigInteger denominator = BigInteger.ONE;
            Point currentPoint = points.get(i);

            for (int j = 0; j < k; j++) {
                if (i == j) {
                    continue;
                }
                Point otherPoint = points.get(j);
                
               
                numerator = numerator.multiply(otherPoint.x.negate());
                
               
                denominator = denominator.multiply(currentPoint.x.subtract(otherPoint.x));
            }
            
            
            BigInteger lagrangeBasis = numerator.divide(denominator);
            
           
            BigInteger term = currentPoint.y.multiply(lagrangeBasis);
            
            
            secret = secret.add(term);
        }

        return secret;
    }

    public static void main(String[] args) {
        // --- Test Case 1 ---
        String testCase1 = "{"
            + "\"keys\": { \"n\": 4, \"k\": 3 },"
            + "\"1\": { \"base\": \"10\", \"value\": \"4\" },"
            + "\"2\": { \"base\": \"2\", \"value\": \"111\" },"
            + "\"3\": { \"base\": \"10\", \"value\": \"12\" },"
            + "\"6\": { \"base\": \"4\", \"value\": \"213\" }"
            + "}";

        // --- Test Case 2 ---
        String testCase2 = "{"
            + "\"keys\": { \"n\": 10, \"k\": 7 },"
            + "\"1\": { \"base\": \"6\", \"value\": \"13444211440455345511\" },"
            + "\"2\": { \"base\": \"15\", \"value\": \"aed7015a346d635\" }," // Corrected value based on your input
            + "\"3\": { \"base\": \"15\", \"value\": \"6aeeb69631c227c\" },"
            + "\"4\": { \"base\": \"16\", \"value\": \"e1b5e05623d881f\" },"
            + "\"5\": { \"base\": \"8\", \"value\": \"316034514573652620673\" },"
            + "\"6\": { \"base\": \"3\", \"value\": \"2122212201122002221120200210011020220200\" },"
            + "\"7\": { \"base\": \"3\", \"value\": \"20120221122211000100210021102001201112121\" },"
            + "\"8\": { \"base\": \"6\", \"value\": \"20220554335330240002224253\" },"
            + "\"9\": { \"base\": \"12\", \"value\": \"45153788322a1255483\" },"
            + "\"10\": { \"base\": \"7\", \"value\": \"1101613130313526312514143\" }"
            + "}";

        try {
            BigInteger secret1 = findSecret(testCase1);
            BigInteger secret2 = findSecret(testCase2);

            System.out.println("Secret for Test Case 1: " + secret1);
            System.out.println("Secret for Test Case 2: " + secret2);
        } catch (Exception e) {
            System.err.println("An error occurred: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
