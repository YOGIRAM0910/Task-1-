# Task-1-
mport re
import math

# List of common passwords (example)
common_passwords = [
    "password", "123456", "123456789", "12345678", "12345",
    "qwerty", "abc123", "111111", "letmein", "1234"
]

# Function to calculate entropy
def calculate_entropy(password):
    length = len(password)
    character_set_size = 0

    # Check for different character sets
    if re.search(r'[a-z]', password):
        character_set_size += 26
    if re.search(r'[A-Z]', password):
        character_set_size += 26
    if re.search(r'[0-9]', password):
        character_set_size += 10
    if re.search(r'[\W_]', password):
        character_set_size += 32  # Approximate for special characters

    if character_set_size == 0:
        return 0

    return length * math.log2(character_set_size)

# Function to assess password strength
def assess_password_strength(password):
    score = 0
    length = len(password)

    # Length-based scoring
    if length >= 12:
        score += 3
    elif length >= 8:
        score += 2
    elif length >= 6:
        score += 1

    # Complexity-based scoring
    complexity_criteria = 0
    if re.search(r'[a-z]', password):
        complexity_criteria += 1
    if re.search(r'[A-Z]', password):
        complexity_criteria += 1
    if re.search(r'[0-9]', password):
        complexity_criteria += 1
    if re.search(r'[\W_]', password):
        complexity_criteria += 1
    
    score += complexity_criteria

    # Uniqueness check
    if password in common_passwords:
        score -= 3  # Significant penalty for common passwords
    elif any(re.match(pattern, password) for pattern in ["123456", "abcdef", "qwerty", "password"]):
        score -= 2  # Penalty for common patterns

    # Entropy scoring
    entropy = calculate_entropy(password)
    if entropy >= 60:
        score += 3
    elif entropy >= 40:
        score += 2
    elif entropy >= 20:
        score += 1

    # Final assessment
    if score >= 8:
        return "Very Strong"
    elif score >= 5:
        return "Strong"
    elif score >= 3:
        return "Moderate"
    else:
        return "Weak"

# Example usage
password = input("Enter a password to assess: ")
strength = assess_password_strength(password)
print(f"Password Strength: {strength}")
