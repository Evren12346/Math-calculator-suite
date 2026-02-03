import re
import math
from fractions import Fraction
from itertools import product
from sympy import symbols, solve, sympify, diff, integrate, limit, simplify, Matrix, oo
from statistics import mean, median, mode, stdev, variance

def evaluate_expression(expr_string, var_values=None):
    """Evaluate an expression by substituting variables with values.
    
    var_values: dictionary with variable names as keys and values as values
    e.g., {'x': 1, 'y': 2}
    """
    try:
        # Create a safe namespace with the variable values
        if var_values is None:
            namespace = {}
        else:
            namespace = var_values.copy()
        result = eval(expr_string, {"__builtins__": {}}, namespace)
        return result
    except Exception as e:
        raise ValueError(f"Error evaluating expression: {e}")

def find_variables(expr_string):
    """Find all variables (x, y, z) in the expression."""
    variables = set(re.findall(r'[xyz]', expr_string))
    if not variables:
        raise ValueError("Expression must contain at least one variable (x, y, or z)")
    return sorted(list(variables))

def check_equivalence(expr1, expr2, test_value_sets):
    """Check if two expressions are equivalent by testing with multiple value combinations."""
    # Find the variables in both expressions
    vars1 = find_variables(expr1)
    vars2 = find_variables(expr2)
    
    if vars1 != vars2:
        raise ValueError(f"Both expressions must use the same variables. Expression 1 has {vars1}, Expression 2 has {vars2}")
    
    results = []
    
    for var_values in test_value_sets:
        try:
            result1 = evaluate_expression(expr1, var_values)
            result2 = evaluate_expression(expr2, var_values)
            
            # Convert to float for comparison (handles Fraction objects)
            result1_float = float(result1)
            result2_float = float(result2)
            
            match = abs(result1_float - result2_float) < 1e-10  # Small tolerance for floating point errors
            results.append({
                'values': var_values,
                'result1': result1,
                'result2': result2,
                'match': match
            })
            
        except Exception as e:
            raise ValueError(f"Error evaluating with {var_values}: {e}")
    
    return results

def equivalence_checker():
    """Run the equivalence checker mode."""
    print("=" * 70)
    print("Expression Equivalence Checker")
    print("=" * 70 + "\n")
    
    try:
        while True:
            expr1 = input("Expression 1 (or 'quit' to exit): ").strip()
            
            if expr1.lower() == 'quit':
                print("\nExiting equivalence checker...\n")
                break
            
            expr2 = input("Expression 2: ").strip()
            
            if not expr1 or not expr2:
                print("Error: Expressions cannot be empty\n")
                continue
            
            try:
                # Find variables in the expressions
                variables = find_variables(expr1)
                find_variables(expr2)  # Validate expr2 also
                # Find variables in the expressions
                variables = find_variables(expr1)
                find_variables(expr2)  # Validate expr2 also
                
                # Comprehensive test values covering every type of number
                test_values = [
                    # Zero
                    0,
                    
                    # Positive integers - small
                    1,
                    2,
                    3,
                    
                    # Positive integers - prime
                    5,
                    7,
                    11,
                    13,
                    
                    # Positive integers - composite
                    4,
                    6,
                    8,
                    9,
                    10,
                    
                    # Positive integers - large
                    100,
                    1000,
                    
                    # Negative integers - small
                    -1,
                    -2,
                    -3,
                    
                    # Negative integers - prime
                    -5,
                    -7,
                    -11,
                    -13,
                    
                    # Negative integers - composite
                    -4,
                    -6,
                    -8,
                    -9,
                    -10,
                    
                    # Negative integers - large
                    -100,
                    -1000,
                    
                    # Positive proper fractions (numerator < denominator)
                    Fraction(1, 2),
                    Fraction(1, 3),
                    Fraction(2, 3),
                    Fraction(1, 4),
                    Fraction(3, 4),
                    Fraction(1, 5),
                    Fraction(2, 5),
                    Fraction(1, 7),
                    
                    # Positive improper fractions (numerator > denominator)
                    Fraction(3, 2),
                    Fraction(5, 2),
                    Fraction(7, 2),
                    Fraction(5, 3),
                    Fraction(7, 3),
                    Fraction(11, 4),
                    Fraction(9, 5),
                    
                    # Negative proper fractions
                    Fraction(-1, 2),
                    Fraction(-1, 3),
                    Fraction(-2, 3),
                    Fraction(-1, 4),
                    Fraction(-3, 4),
                    Fraction(-1, 5),
                    Fraction(-2, 5),
                    
                    # Negative improper fractions
                    Fraction(-3, 2),
                    Fraction(-5, 2),
                    Fraction(-7, 2),
                    Fraction(-5, 3),
                    Fraction(-7, 3),
                    Fraction(-11, 4),
                    
                    # Very small positive fractions
                    Fraction(1, 10),
                    Fraction(1, 100),
                    Fraction(1, 1000),
                    
                    # Very small negative fractions
                    Fraction(-1, 10),
                    Fraction(-1, 100),
                    Fraction(-1, 1000),
                ]
                
                # Generate all combinations of test values for all variables
                test_value_sets = [dict(zip(variables, combo)) for combo in product(test_values, repeat=len(variables))]
                
                print("\nTesting...\n")
                
                results = check_equivalence(expr1, expr2, test_value_sets)
                
                # Check results
                all_match = all(result['match'] for result in results)
                
                print("=" * 70)
                print(f"Expression 1: {expr1}")
                print(f"Expression 2: {expr2}")
                print("=" * 70)
                print()
                
                # Report findings
                if all_match:
                    print("✓ RESULT: EQUIVALENT")
                else:
                    print("✗ RESULT: NOT EQUIVALENT")
                
                print("\n" + "=" * 70 + "\n")
                
            except ValueError as e:
                print(f"Error: {e}\n")
    except Exception as e:
        print(f"Unexpected error: {e}\n")
    except KeyboardInterrupt:
        print("\n\nExiting equivalence checker...\n")

def calculator():
    """Run the calculator mode."""
    print("=" * 70)
    print("Calculator")
    print("=" * 70 + "\n")
    
    try:
        while True:
            expression = input("Enter expression (or 'quit' to exit): ").strip()
            
            if expression.lower() == 'quit':
                print("\nExiting calculator...\n")
                break
            
            if not expression:
                print("Error: Expression cannot be empty\n")
                continue
            
            try:
                result = evaluate_expression(expression)
                print(f"= {result}\n")
            except ValueError as e:
                print(f"Error: {e}\n")
            except Exception as e:
                print(f"Error: {e}\n")
    
    except KeyboardInterrupt:
        print("\n\nExiting calculator...\n")

def gcd(a, b):
    """Calculate the greatest common divisor using Euclidean algorithm."""
    a, b = abs(a), abs(b)
    while b:
        a, b = b, a % b
    return a

def lcm(a, b):
    """Calculate the least common multiple."""
    return abs(a * b) // gcd(a, b)

def gcd_lcm_calculator():
    """Run the GCD/LCM calculator mode."""
    print("=" * 70)
    print("GCD/LCM Calculator")
    print("=" * 70 + "\n")
    
    try:
        while True:
            user_input = input("Enter two numbers separated by comma (or 'quit' to exit): ").strip()
            
            if user_input.lower() == 'quit':
                print("\nExiting GCD/LCM calculator...\n")
                break
            
            if not user_input:
                print("Error: Input cannot be empty\n")
                continue
            
            try:
                if ',' not in user_input:
                    print("Error: Please enter two numbers separated by a comma\n")
                    continue
                
                parts = user_input.split(',')
                if len(parts) != 2:
                    print("Error: Please enter exactly two numbers\n")
                    continue
                
                num1 = int(parts[0].strip())
                num2 = int(parts[1].strip())
                
                if num1 == 0 or num2 == 0:
                    print("Error: Numbers cannot be zero\n")
                    continue
                
                gcd_result = gcd(num1, num2)
                lcm_result = lcm(num1, num2)
                
                print(f"\nNumbers: {num1}, {num2}")
                print(f"GCD (Greatest Common Divisor): {gcd_result}")
                print(f"LCM (Least Common Multiple):   {lcm_result}\n")
                
            except ValueError:
                print("Error: Please enter valid integers\n")
            except Exception as e:
                print(f"Error: {e}\n")
    
    except KeyboardInterrupt:
        print("\n\nExiting GCD/LCM calculator...\n")

def equation_solver():
    """Run the equation solver mode."""
    print("=" * 70)
    print("Equation Solver")
    print("=" * 70)
    print("Solve equations for a variable (e.g., '2*x + 3 = 5' for x)\n")
    
    try:
        while True:
            equation_str = input("Enter equation (use = to separate sides, or 'quit' to exit): ").strip()
            
            if equation_str.lower() == 'quit':
                print("\nExiting equation solver...\n")
                break
            
            if not equation_str or '=' not in equation_str:
                print("Error: Please enter a valid equation with '=' sign\n")
                continue
            
            variable_str = input("Enter variable to solve for (x, y, or z): ").strip()
            
            if not variable_str or variable_str not in ['x', 'y', 'z']:
                print("Error: Please enter a valid variable (x, y, or z)\n")
                continue
            
            try:
                # Split equation into left and right sides
                left, right = equation_str.split('=', 1)
                
                # Create sympy equation
                var = symbols(variable_str)
                left_expr = sympify(left.strip())
                right_expr = sympify(right.strip())
                
                # Solve the equation
                solutions = solve(left_expr - right_expr, var)
                
                print("\n" + "=" * 70)
                print(f"Equation: {equation_str}")
                print(f"Solving for: {variable_str}")
                print("=" * 70)
                print()
                
                if not solutions:
                    print("No solutions found (equation may have no solution or infinite solutions)")
                elif len(solutions) == 1:
                    print(f"✓ Solution: {variable_str} = {solutions[0]}")
                else:
                    print(f"✓ Solutions found ({len(solutions)}):")
                    for i, sol in enumerate(solutions, 1):
                        print(f"  {i}. {variable_str} = {sol}")
                
                print("\n" + "=" * 70 + "\n")
                
            except Exception as e:
                print(f"Error: Could not solve equation: {e}\n")
    
    except KeyboardInterrupt:
        print("\n\nExiting equation solver...\n")

def factorial_calculator():
    """Run the factorial calculator mode."""
    print("=" * 70)
    print("Factorial & Combinations Calculator")
    print("=" * 70 + "\n")
    
    try:
        while True:
            print("Options:")
            print("  1) Calculate factorial (n!)")
            print("  2) Calculate combinations (nCr)")
            print("  3) Calculate permutations (nPr)")
            print()
            
            choice = input("Select option (1-3, or 'quit' to exit): ").strip()
            
            if choice.lower() == 'quit':
                print("\nExiting factorial calculator...\n")
                break
            
            if choice == '1':
                try:
                    n = int(input("Enter n for n!: ").strip())
                    if n < 0:
                        print("Error: Factorial not defined for negative numbers\n")
                        continue
                    result = math.factorial(n)
                    print(f"✓ {n}! = {result}\n")
                except ValueError:
                    print("Error: Please enter a valid integer\n")
            
            elif choice == '2':
                try:
                    n = int(input("Enter n: ").strip())
                    r = int(input("Enter r: ").strip())
                    if n < 0 or r < 0 or r > n:
                        print("Error: Invalid values (n >= r >= 0)\n")
                        continue
                    result = math.comb(n, r)
                    print(f"✓ C({n},{r}) = {result}\n")
                except ValueError:
                    print("Error: Please enter valid integers\n")
            
            elif choice == '3':
                try:
                    n = int(input("Enter n: ").strip())
                    r = int(input("Enter r: ").strip())
                    if n < 0 or r < 0 or r > n:
                        print("Error: Invalid values (n >= r >= 0)\n")
                        continue
                    result = math.perm(n, r)
                    print(f"✓ P({n},{r}) = {result}\n")
                except ValueError:
                    print("Error: Please enter valid integers\n")
            
            else:
                print("Invalid choice. Please enter 1-3 or 'quit'.\n")
    
    except KeyboardInterrupt:
        print("\n\nExiting factorial calculator...\n")

def quadratic_solver():
    """Run the quadratic formula solver mode."""
    print("=" * 70)
    print("Quadratic Formula Solver")
    print("=" * 70)
    print("Solve ax² + bx + c = 0\n")
    
    try:
        while True:
            a_input = input("Enter coefficient a (or 'quit' to exit): ").strip()
            
            if a_input.lower() == 'quit':
                print("\nExiting quadratic solver...\n")
                break
            
            try:
                a = float(a_input)
                b = float(input("Enter coefficient b: ").strip())
                c = float(input("Enter coefficient c: ").strip())
                
                if a == 0:
                    print("Error: a cannot be zero for a quadratic equation\n")
                    continue
            except ValueError:
                print("Error: Please enter valid numbers\n")
                continue
        
            discriminant = b**2 - 4*a*c
            
            print("\n" + "=" * 70)
            print(f"Equation: {a}x² + {b}x + {c} = 0")
            print("=" * 70)
            print()
            
            if discriminant > 0:
                x1 = (-b + math.sqrt(discriminant)) / (2*a)
                x2 = (-b - math.sqrt(discriminant)) / (2*a)
                print(f"✓ Two real solutions:")
                print(f"  x₁ = {x1}")
                print(f"  x₂ = {x2}")
            elif discriminant == 0:
                x = -b / (2*a)
                print(f"✓ One real solution (repeated root):")
                print(f"  x = {x}")
            else:
                real_part = -b / (2*a)
                imag_part = math.sqrt(abs(discriminant)) / (2*a)
                print(f"✓ Two complex solutions:")
                print(f"  x₁ = {real_part} + {imag_part}i")
                print(f"  x₂ = {real_part} - {imag_part}i")
            
            print("\n" + "=" * 70 + "\n")
        
    except KeyboardInterrupt:
        print("\n\nExiting quadratic solver...\n")

def system_of_equations_solver():
    """Run the system of equations solver mode."""
    print("=" * 70)
    print("System of Equations Solver")
    print("=" * 70)
    print("Solve a system of linear equations\n")
    
    try:
        while True:
            num_input = input("How many equations (or 'quit' to exit)? ").strip()
            
            if num_input.lower() == 'quit':
                print("\nExiting system solver...\n")
                break
            
            try:
                num_equations = int(num_input)
                
                if num_equations < 2:
                    print("Error: Need at least 2 equations\n")
                    continue
            except ValueError:
                print("Error: Please enter a valid number\n")
                continue
            
            equations = []
            for i in range(num_equations):
                eq = input(f"Enter equation {i+1} (e.g., 2*x + 3*y = 5): ").strip()
                equations.append(eq)
            
            try:
                # Parse equations
                parsed_equations = []
                for eq in equations:
                    if '=' not in eq:
                        print("Error: Invalid equation format\n")
                        break
                    left, right = eq.split('=', 1)
                    parsed_equations.append(sympify(left.strip()) - sympify(right.strip()))
                
                if len(parsed_equations) != len(equations):
                    continue
                
                # Find variables
                x, y, z = symbols('x y z')
                all_vars = set()
                for eq in parsed_equations:
                    all_vars.update(eq.free_symbols)
                
                # Solve the system
                solutions = solve(parsed_equations, list(all_vars))
                
                print("\n" + "=" * 70)
                print("System of Equations:")
                for i, eq in enumerate(equations, 1):
                    print(f"  {i}. {eq}")
                print("=" * 70)
                print()
                
                if not solutions:
                    print("No solutions found (system may be inconsistent)")
                elif isinstance(solutions, dict):
                    print("✓ Solution found:")
                    for var, val in solutions.items():
                        print(f"  {var} = {val}")
                else:
                    print(f"✓ Solutions: {solutions}")
                
                print("\n" + "=" * 70 + "\n")
                
            except Exception as e:
                print(f"Error: Could not solve system: {e}\n")
    
    except KeyboardInterrupt:
        print("\n\nExiting system solver...\n")

def expression_simplifier():
    """Run the expression simplifier mode."""
    print("=" * 70)
    print("Expression Simplifier")
    print("=" * 70 + "\n")
    
    try:
        while True:
            expression = input("Enter expression to simplify (or 'quit' to exit): ").strip()
            
            if expression.lower() == 'quit':
                print("\nExiting expression simplifier...\n")
                break
            
            if not expression:
                print("Error: Expression cannot be empty\n")
                continue
            
            try:
                expr = sympify(expression)
                simplified = simplify(expr)
                
                print("\n" + "=" * 70)
                print(f"Original:   {expr}")
                print(f"Simplified: {simplified}")
                print("=" * 70 + "\n")
                
            except Exception as e:
                print(f"Error: Could not simplify expression: {e}\n")
    
    except KeyboardInterrupt:
        print("\n\nExiting expression simplifier...\n")

def derivative_calculator():
    """Run the derivative calculator mode."""
    print("=" * 70)
    print("Derivative Calculator")
    print("=" * 70 + "\n")
    
    try:
        while True:
            expression = input("Enter expression (or 'quit' to exit): ").strip()
            
            if expression.lower() == 'quit':
                print("\nExiting derivative calculator...\n")
                break
            
            variable = input("Enter variable to differentiate with respect to (x, y, z): ").strip()
            
            if not expression or variable not in ['x', 'y', 'z']:
                print("Error: Invalid input\n")
                continue
            
            try:
                var = symbols(variable)
                expr = sympify(expression)
                derivative = diff(expr, var)
                
                print("\n" + "=" * 70)
                print(f"Expression: {expr}")
                print(f"Variable:   {variable}")
                print(f"Derivative: {derivative}")
                print("=" * 70 + "\n")
                
            except Exception as e:
                print(f"Error: Could not compute derivative: {e}\n")
    
    except KeyboardInterrupt:
        print("\n\nExiting derivative calculator...\n")

def integral_calculator():
    """Run the integral calculator mode."""
    print("=" * 70)
    print("Integral Calculator")
    print("=" * 70)
    print("Calculate indefinite integrals\n")
    
    try:
        while True:
            expression = input("Enter expression (or 'quit' to exit): ").strip()
            
            if expression.lower() == 'quit':
                print("\nExiting integral calculator...\n")
                break
            
            variable = input("Enter variable to integrate with respect to (x, y, z): ").strip()
            
            if not expression or variable not in ['x', 'y', 'z']:
                print("Error: Invalid input\n")
                continue
            
            try:
                var = symbols(variable)
                expr = sympify(expression)
                integral = integrate(expr, var)
                
                print("\n" + "=" * 70)
                print(f"Expression: {expr}")
                print(f"Variable:   {variable}")
                print(f"Integral:   {integral} + C")
                print("=" * 70 + "\n")
                
            except Exception as e:
                print(f"Error: Could not compute integral: {e}\n")
    
    except KeyboardInterrupt:
        print("\n\nExiting integral calculator...\n")

def limit_calculator():
    """Run the limit calculator mode."""
    print("=" * 70)
    print("Limit Calculator")
    print("=" * 70 + "\n")
    
    try:
        while True:
            expression = input("Enter expression (or 'quit' to exit): ").strip()
            
            if expression.lower() == 'quit':
                print("\nExiting limit calculator...\n")
                break
            
            variable = input("Enter variable (x, y, z): ").strip()
            point = input("Enter limit point (or 'inf' for infinity): ").strip()
            
            if not expression or variable not in ['x', 'y', 'z']:
                print("Error: Invalid input\n")
                continue
            
            try:
                var = symbols(variable)
                expr = sympify(expression)
                
                if point.lower() == 'inf':
                    point_val = oo
                else:
                    point_val = sympify(point)
                
                limit_result = limit(expr, var, point_val)
                
                print("\n" + "=" * 70)
                print(f"Expression: {expr}")
                print(f"Variable:   {variable}")
                print(f"Limit as {variable} → {point}")
                print(f"Limit:      {limit_result}")
                print("=" * 70 + "\n")
                
            except Exception as e:
                print(f"Error: Could not compute limit: {e}\n")
    
    except KeyboardInterrupt:
        print("\n\nExiting limit calculator...\n")

def prime_checker():
    """Run the prime number checker mode."""
    print("=" * 70)
    print("Prime Number Checker")
    print("=" * 70 + "\n")
    
    def is_prime(n):
        if not isinstance(n, int) or n < 2:
            return False
        if n == 2:
            return True
        if n % 2 == 0:
            return False
        for i in range(3, int(math.sqrt(n)) + 1, 2):
            if n % i == 0:
                return False
        return True
    
    try:
        while True:
            user_input = input("Enter a number to check (or 'quit' to exit): ").strip()
            
            if user_input.lower() == 'quit':
                print("\nExiting prime checker...\n")
                break
            
            if not user_input:
                print("Error: Input cannot be empty\n")
                continue
            
            try:
                num = int(user_input)
                if is_prime(num):
                    print(f"✓ {num} IS a prime number\n")
                else:
                    print(f"✗ {num} is NOT a prime number\n")
            except ValueError:
                print("Error: Please enter a valid integer\n")
    
    except KeyboardInterrupt:
        print("\n\nExiting prime checker...\n")

def fraction_simplifier():
    """Run the fraction simplifier mode."""
    print("=" * 70)
    print("Fraction Simplifier")
    print("=" * 70 + "\n")
    
    try:
        while True:
            user_input = input("Enter fraction as 'numerator/denominator' (or 'quit' to exit): ").strip()
            
            if user_input.lower() == 'quit':
                print("\nExiting fraction simplifier...\n")
                break
            
            if not user_input:
                print("Error: Input cannot be empty\n")
                continue
            
            try:
                if '/' not in user_input:
                    print("Error: Please enter fraction in format 'numerator/denominator'\n")
                    continue
                
                parts = user_input.split('/')
                if len(parts) != 2:
                    print("Error: Invalid fraction format\n")
                    continue
                
                numerator = int(parts[0].strip())
                denominator = int(parts[1].strip())
                
                if denominator == 0:
                    print("Error: Denominator cannot be zero\n")
                    continue
                
                frac = Fraction(numerator, denominator)
                print(f"Simplified: {frac}\n")
            except ValueError:
                print("Error: Please enter valid integers\n")
    
    except KeyboardInterrupt:
        print("\n\nExiting fraction simplifier...\n")

def matrix_calculator():
    """Run the matrix calculator mode."""
    print("=" * 70)
    print("Matrix Calculator")
    print("=" * 70 + "\n")
    
    try:
        while True:
            print("Options:")
            print("  1) Add two matrices")
            print("  2) Multiply two matrices")
            print("  3) Calculate determinant")
            print("  4) Calculate inverse")
            print()
            
            choice = input("Select option (1-4, or 'quit' to exit): ").strip()
            
            if choice.lower() == 'quit':
                print("\nExiting matrix calculator...\n")
                break
            
            if choice == '1':
                try:
                    m1_input = input("Enter first matrix as [[a,b],[c,d]]: ").strip()
                    m2_input = input("Enter second matrix as [[a,b],[c,d]]: ").strip()
                    
                    m1 = Matrix(eval(m1_input))
                    m2 = Matrix(eval(m2_input))
                    result = m1 + m2
                    
                    print(f"Result:\n{result}\n")
                except Exception as e:
                    print(f"Error: {e}\n")
            
            elif choice == '2':
                try:
                    m1_input = input("Enter first matrix as [[a,b],[c,d]]: ").strip()
                    m2_input = input("Enter second matrix as [[a,b],[c,d]]: ").strip()
                    
                    m1 = Matrix(eval(m1_input))
                    m2 = Matrix(eval(m2_input))
                    result = m1 * m2
                    
                    print(f"Result:\n{result}\n")
                except Exception as e:
                    print(f"Error: {e}\n")
            
            elif choice == '3':
                try:
                    m_input = input("Enter matrix as [[a,b],[c,d]]: ").strip()
                    m = Matrix(eval(m_input))
                    det = m.det()
                    
                    print(f"Determinant: {det}\n")
                except Exception as e:
                    print(f"Error: {e}\n")
            
            elif choice == '4':
                try:
                    m_input = input("Enter matrix as [[a,b],[c,d]]: ").strip()
                    m = Matrix(eval(m_input))
                    inv = m.inv()
                    
                    print(f"Inverse:\n{inv}\n")
                except Exception as e:
                    print(f"Error: Could not calculate inverse: {e}\n")
            
            else:
                print("Invalid choice. Please enter 1-4 or 'quit'.\n")
    
    except KeyboardInterrupt:
        print("\n\nExiting matrix calculator...\n")

def statistics_calculator():
    """Run the statistics calculator mode."""
    print("=" * 70)
    print("Statistics Calculator")
    print("=" * 70 + "\n")
    
    try:
        user_input = input("Enter numbers separated by commas (e.g., 1,2,3,4,5): ").strip()
        
        if not user_input:
            print("Error: Input cannot be empty\n")
            return
        
        try:
            numbers = [float(x.strip()) for x in user_input.split(',')]
            
            if len(numbers) == 0:
                print("Error: No numbers provided\n")
                return
            
            print("\n" + "=" * 70)
            print("Dataset:", numbers)
            print("=" * 70)
            print()
            
            print(f"Count:              {len(numbers)}")
            print(f"Mean (Average):     {mean(numbers)}")
            print(f"Median:             {median(numbers)}")
            
            try:
                print(f"Mode:               {mode(numbers)}")
            except:
                print(f"Mode:               No unique mode")
            
            if len(numbers) > 1:
                print(f"Standard Deviation: {stdev(numbers)}")
                print(f"Variance:           {variance(numbers)}")
                
                sorted_nums = sorted(numbers)
                print(f"Min:                {sorted_nums[0]}")
                print(f"Max:                {sorted_nums[-1]}")
                print(f"Range:              {sorted_nums[-1] - sorted_nums[0]}")
            
            print("\n" + "=" * 70 + "\n")
            
        except ValueError:
            print("Error: Please enter valid numbers separated by commas\n")
    
    except KeyboardInterrupt:
        print("\n\nExiting statistics calculator...\n")

def unit_converter():
    """Run the unit converter mode."""
    print("=" * 70)
    print("Unit Converter")
    print("=" * 70 + "\n")
    
    conversions = {
        'length': {
            'mm_to_cm': 0.1,
            'cm_to_m': 0.01,
            'm_to_km': 0.001,
            'in_to_cm': 2.54,
            'ft_to_m': 0.3048,
            'mi_to_km': 1.60934
        },
        'weight': {
            'g_to_kg': 0.001,
            'kg_to_g': 1000,
            'lb_to_kg': 0.453592,
            'oz_to_g': 28.3495
        },
        'temperature': {
            'c_to_f': lambda x: (x * 9/5) + 32,
            'f_to_c': lambda x: (x - 32) * 5/9,
            'c_to_k': lambda x: x + 273.15,
            'k_to_c': lambda x: x - 273.15
        }
    }
    
    try:
        while True:
            print("Categories: length, weight, temperature")
            category = input("Select category (or 'quit' to exit): ").strip().lower()
            
            if category == 'quit':
                print("\nExiting unit converter...\n")
                break
            
            if category not in conversions:
                print("Error: Invalid category\n")
                continue
            
            print(f"\nAvailable conversions in {category}:")
            for i, conv in enumerate(conversions[category].keys(), 1):
                print(f"  {i}. {conv}")
            
            conversion = input("Enter conversion (e.g., 'mm_to_cm'): ").strip()
            
            if conversion not in conversions[category]:
                print("Error: Invalid conversion\n")
                continue
            
            try:
                value = float(input("Enter value to convert: ").strip())
                
                conv_func = conversions[category][conversion]
                if callable(conv_func):
                    result = conv_func(value)
                else:
                    result = value * conv_func
                
                print(f"\n{value} {conversion.split('_')[0]} = {result} {conversion.split('_')[2]}\n")
            except ValueError:
                print("Error: Please enter valid input\n")
        
    except KeyboardInterrupt:
        print("\n\nExiting unit converter...\n")

def number_system_converter():
    """Run the number system converter mode."""
    print("=" * 70)
    print("Number System Converter")
    print("=" * 70 + "\n")
    
    try:
        while True:
            print("Convert from: decimal, binary, hexadecimal, octal")
            from_system = input("Convert from (or 'quit' to exit): ").strip().lower()
            
            if from_system == 'quit':
                print("\nExiting number system converter...\n")
                break
            
            to_system = input("Convert to: ").strip().lower()
            value = input("Enter value: ").strip()
            
            systems = {'decimal': 10, 'binary': 2, 'hexadecimal': 16, 'octal': 8}
            
            if from_system not in systems or to_system not in systems:
                print("Error: Invalid number system\n")
                continue
            
            try:
                # Convert to decimal first
                if from_system == 'decimal':
                    decimal_val = int(value)
                elif from_system == 'binary':
                    decimal_val = int(value, 2)
                elif from_system == 'hexadecimal':
                    decimal_val = int(value, 16)
                else:  # octal
                    decimal_val = int(value, 8)
                
                # Convert to target system
                if to_system == 'decimal':
                    result = str(decimal_val)
                elif to_system == 'binary':
                    result = bin(decimal_val)[2:]
                elif to_system == 'hexadecimal':
                    result = hex(decimal_val)[2:]
                else:  # octal
                    result = oct(decimal_val)[2:]
                
                print(f"\n{value} ({from_system}) = {result} ({to_system})\n")
            
            except ValueError:
                print("Error: Please enter valid input\n")
        
    except KeyboardInterrupt:
        print("\n\nExiting number system converter...\n")

def trig_calculator():
    """Run the trigonometry calculator mode."""
    print("=" * 70)
    print("Trigonometry Calculator")
    print("=" * 70 + "\n")
    
    try:
        while True:
            angle_input = input("Enter angle in degrees (or 'quit' to exit): ").strip()
            
            if angle_input.lower() == 'quit':
                print("\nExiting trig calculator...\n")
                break
            
            try:
                angle = float(angle_input)
                angle_rad = math.radians(angle)
                
                print("\n" + "=" * 70)
                print(f"Angle: {angle}° ({angle_rad:.4f} radians)")
                print("=" * 70)
                print()
                
                print(f"sin({angle}°) = {math.sin(angle_rad):.6f}")
                print(f"cos({angle}°) = {math.cos(angle_rad):.6f}")
                print(f"tan({angle}°) = {math.tan(angle_rad):.6f}")
                print(f"cot({angle}°) = {1/math.tan(angle_rad):.6f}" if math.sin(angle_rad) != 0 else "cot: undefined")
                print(f"sec({angle}°) = {1/math.cos(angle_rad):.6f}" if math.cos(angle_rad) != 0 else "sec: undefined")
                print(f"csc({angle}°) = {1/math.sin(angle_rad):.6f}" if math.sin(angle_rad) != 0 else "csc: undefined")
                
                print("\n" + "=" * 70 + "\n")
            
            except ValueError:
                print("Error: Please enter a valid number\n")
        
    except KeyboardInterrupt:
        print("\n\nExiting trig calculator...\n")

def angle_converter():
    """Run the angle converter mode."""
    print("=" * 70)
    print("Angle Converter")
    print("=" * 70 + "\n")
    
    try:
        while True:
            angle_input = input("Enter angle value (or 'quit' to exit): ").strip()
            
            if angle_input.lower() == 'quit':
                print("\nExiting angle converter...\n")
                break
            
            try:
                angle = float(angle_input)
                from_unit = input("From (degrees/radians): ").strip().lower()
                to_unit = input("To (degrees/radians): ").strip().lower()
                
                if from_unit == 'degrees' and to_unit == 'radians':
                    result = math.radians(angle)
                    print(f"\n{angle}° = {result:.6f} radians\n")
                elif from_unit == 'radians' and to_unit == 'degrees':
                    result = math.degrees(angle)
                    print(f"\n{angle} radians = {result:.6f}°\n")
                else:
                    print("Error: Please enter 'degrees' or 'radians'\n")
            
            except ValueError:
                print("Error: Please enter a valid number\n")
        
    except KeyboardInterrupt:
        print("\n\nExiting angle converter...\n")

def display_menu():
    """Display the main menu."""
    print("\n" + "=" * 70)
    print("Math Tools - Comprehensive Calculator Suite")
    print("=" * 70)
    print()
    print("  1) Expression Equivalence Checker")
    print("  2) Calculator")
    print("  3) Equation Solver")
    print("  4) Quadratic Formula Solver")
    print("  5) System of Equations Solver")
    print("  6) Expression Simplifier")
    print("  7) GCD/LCM Calculator")
    print("  8) Factorial & Combinations Calculator")
    print("  9) Prime Number Checker")
    print(" 10) Fraction Simplifier")
    print(" 11) Derivative Calculator")
    print(" 12) Integral Calculator")
    print(" 13) Limit Calculator")
    print(" 14) Matrix Calculator")
    print(" 15) Statistics Calculator")
    print(" 16) Unit Converter")
    print(" 17) Number System Converter")
    print(" 18) Trigonometry Calculator")
    print(" 19) Angle Converter")
    print()
    print("=" * 70 + "\n")

def main():
    """Main entry point."""
    while True:
        display_menu()
        
        choice = input("Select option (1-19, or 'quit' to exit): ").strip()
        
        if choice.lower() == 'quit':
            print("\nExiting Math Tools...\n")
            break
        elif choice == '1':
            equivalence_checker()
        elif choice == '2':
            calculator()
        elif choice == '3':
            equation_solver()
        elif choice == '4':
            quadratic_solver()
        elif choice == '5':
            system_of_equations_solver()
        elif choice == '6':
            expression_simplifier()
        elif choice == '7':
            gcd_lcm_calculator()
        elif choice == '8':
            factorial_calculator()
        elif choice == '9':
            prime_checker()
        elif choice == '10':
            fraction_simplifier()
        elif choice == '11':
            derivative_calculator()
        elif choice == '12':
            integral_calculator()
        elif choice == '13':
            limit_calculator()
        elif choice == '14':
            matrix_calculator()
        elif choice == '15':
            statistics_calculator()
        elif choice == '16':
            unit_converter()
        elif choice == '17':
            number_system_converter()
        elif choice == '18':
            trig_calculator()
        elif choice == '19':
            angle_converter()
        else:
            print("Invalid choice. Please enter 1-19 or 'quit'.\n")

if __name__ == "__main__":
    main()
