# Stack-InfixtoPostfix-java

import java.util.*;
class Solution {
    static int prec(char ch) {
        if (ch == '^') return 3;
        else if (ch == '*' || ch == '/') return 2;
        else if (ch == '+' || ch == '-') return 1;
        else return -1;
    }

    public static String infixToPostfix(String s) {
        StringBuilder result = new StringBuilder();
        Stack<Character> st = new Stack<>();
        
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            
            // If operand, append to result
            if (Character.isLetterOrDigit(ch)) {
                result.append(ch);
            }
            // If '(', push onto stack
            else if (ch == '(') {
                st.push(ch);
            }
            // If ')', pop until '('
            else if (ch == ')') {
                while (!st.isEmpty() && st.peek() != '(') {
                    result.append(st.pop());
                }
                st.pop(); // Remove '('
            }
            // If operator, pop higher/equal precedence operators and push current operator
            else {
                while (!st.isEmpty() && prec(ch) <= prec(st.peek())) {
                    result.append(st.pop());
                }
                st.push(ch);
            }
        }

        // Pop remaining operators in the stack
        while (!st.isEmpty()) {
            result.append(st.pop());
        }

        return result.toString();
    }

    public static void main(String[] args) {
        String expression = "A+B*(C^D-E)";
        System.out.println("Postfix: " + infixToPostfix(expression));
    }
}
