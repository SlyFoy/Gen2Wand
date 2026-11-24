try:
 from UIKit import UIPasteboard
except ImportError:
    UIPasteboard = None

KEY = (0x62, 0x6A, 0xB7, 0xC1)


def copy_to_clipboard(text: str) -> bool:
    if UIPasteboard is not None:
        UIPasteboard.generalPasteboard.string = text
        return True
    return False


def calc_pwd(uid_hex: str) -> bytes:
    cleaned = uid_hex.replace(" ", "").replace(":", "").replace("-", "").strip()
    uid = bytes.fromhex(cleaned)
    if len(uid) != 7:
        raise ValueError(f"Expected 7-byte UID, got {len(uid)} bytes.")
    # Reverses the last 4 bytes: uid[6], uid[5], uid[4], uid[3]
    tail_rev = uid[6:2:-1]
    return bytes(tail_rev[i] ^ KEY[i] for i in range(4))


def main():
    print("Wand NFC Auth Calculator (Reversed Tail Bytes)")
    print("Type 'q' to quit.\n")

    while True:
        raw = input("Enter 7-byte UID: ").strip()
        if raw.lower() in ("q", "quit", "exit"):
            break

        try:
            pwd = calc_pwd(raw)
            pwd_str = " ".join(f"{b:02X}" for b in pwd)
            auth_cmd = f"1B {pwd_str}"

            print(f"PWD:      {pwd_str}")
            print(f"Auth Cmd: {auth_cmd}")

            if copy_to_clipboard(auth_cmd):
                print("(Copied Auth Cmd to clipboard)\n")
            else:
                print("(Clipboard not available here)\n")
        except ValueError as e:
            print(f"Error: {e}\n")


if __name__ == "__main__":
    main()
