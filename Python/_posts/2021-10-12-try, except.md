```python
def isNumber(s):
    try: int(s)
    except: return "s에 문자가 포함됨."
    return "s는 숫자임."
```