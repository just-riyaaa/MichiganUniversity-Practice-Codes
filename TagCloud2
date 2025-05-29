import string
from htmlFunctions import make_HTML_word, make_HTML_box, print_HTML_file

def read_stop_words(filename="stopWords.txt"):
    with open(filename, 'r') as f:
        return set(word.strip().lower() for word in f)

def parse_debate(filename, stopwords):
    obama_words = []
    romney_words = []
    current_speaker = None

    with open(filename, 'r') as file:
        for line in file:
            line = line.strip()

            # Detect speaker change
            if line.startswith("PRESIDENT OBAMA:"):
                current_speaker = "OBAMA"
                line = line[len("PRESIDENT OBAMA:"):].strip()
            elif line.startswith("MR. ROMNEY:") or line.startswith("GOVERNOR ROMNEY:"):
                current_speaker = "ROMNEY"
                line = line.split(":", 1)[-1].strip()
            elif ":" in line:
                current_speaker = None  # Unknown speaker (moderator)

            if current_speaker:
                # Clean line
                line = line.lower().translate(str.maketrans('', '', string.punctuation))
                words = [word for word in line.split() if word not in stopwords]

                if current_speaker == "OBAMA":
                    obama_words.extend(words)
                elif current_speaker == "ROMNEY":
                    romney_words.extend(words)

    return obama_words, romney_words

def build_word_counts(words):
    count_dict = {}
    for word in words:
        count_dict[word] = count_dict.get(word, 0) + 1
    return count_dict

def top_n_words(count_dict, n=40):
    return sorted(count_dict.items(), key=lambda x: (-x[1], x[0]))[:n]

def build_tag_cloud(word_list):
    if not word_list:
        return ""

    counts = [count for word, count in word_list]
    high, low = max(counts), min(counts)

    sorted_words = sorted(word_list, key=lambda x: x[0])  # Alphabetical
    cloud = ""

    for word, count in sorted_words:
        cloud += make_HTML_word(word, count, high, low) + " "
    
    return cloud.strip()

def main():
    stopwords = read_stop_words()

    debate_file = input("Enter the transcript filename (e.g., debate.txt): ")
    obama_words, romney_words = parse_debate(debate_file, stopwords)

    obama_counts = build_word_counts(obama_words)
    romney_counts = build_word_counts(romney_words)

    obama_top = top_n_words(obama_counts)
    romney_top = top_n_words(romney_counts)

    print("\nObama Top 40:")
    for word, count in obama_top:
        print(f"{count:3} : {word}")

    print("\nRomney Top 40:")
    for word, count in romney_top:
        print(f"{count:3} : {word}")

    # Generate HTML tag clouds
    obama_cloud = build_tag_cloud(obama_top)
    romney_cloud = build_tag_cloud(romney_top)

    obama_html = make_HTML_box(obama_cloud)
    romney_html = make_HTML_box(romney_cloud)

    print_HTML_file(obama_html, "obama")
    print_HTML_file(romney_html, "romney")

    print("\nHTML files 'obama.html' and 'romney.html' created!")

if __name__ == "__main__":
    main()
