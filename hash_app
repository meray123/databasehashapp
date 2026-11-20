#!/usr/bin/env python3

import sys


def hash_text(text):
    hash_value = 14695981039346656037

    for character in text.encode("utf-8"):
        hash_value = hash_value ^ character
        hash_value = (hash_value * 1099511628211) % (2 ** 64)

    return hash_value


def get_file_length(file_name):
    try:
        with open(file_name, "r", encoding="utf-8") as bucket_file:
            return len(bucket_file.read())
    except FileNotFoundError:
        return 0


def save_text(bucket_number, user_text, max_characters_per_file):
    file_name = str(bucket_number) + ".txt"
    index_file = str(bucket_number) + "_index.txt"

    overflow_number = 0

    try:
        with open(index_file, "r", encoding="utf-8") as index:
            overflow_number = int(index.read())
    except FileNotFoundError:
        pass

    if overflow_number == 0:
        last_file = file_name
    else:
        last_file = str(bucket_number) + "_" + str(overflow_number) + ".txt"

    current_length = get_file_length(last_file)

    if current_length + len(user_text) > max_characters_per_file:
        overflow_number += 1
        last_file = str(bucket_number) + "_" + str(overflow_number) + ".txt"

        with open(index_file, "w", encoding="utf-8") as index:
            index.write(str(overflow_number))

    with open(last_file, "a", encoding="utf-8") as bucket_file:
        bucket_file.write(user_text)

    return last_file


def main():
    if len(sys.argv) != 3:
        print("Usage: hash_app <n> <s>")
        sys.exit(1)

    try:
        number_of_buckets = int(sys.argv[1])
        max_characters_per_file = int(sys.argv[2])
    except ValueError:
        print("Both n and s must be whole numbers.")
        sys.exit(1)

    if number_of_buckets <= 0 or max_characters_per_file <= 0:
        print("Both n and s must be positive numbers.")
        sys.exit(1)

    while True:
        try:
            user_text = input("Please enter the string: ")
        except (EOFError, KeyboardInterrupt):
            print()
            break

        if user_text == "":
            continue

        if len(user_text) > max_characters_per_file:
            print("The string is longer than the maximum bucket size.")
            continue

        hash_value = hash_text(user_text)
        bucket_number = (hash_value % number_of_buckets) + 1

        file_name = save_text(
            bucket_number,
            user_text,
            max_characters_per_file
        )

        print(f"{user_text} added to {file_name}")


if __name__ == "__main__":
    main()