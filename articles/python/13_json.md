# json

## 写入json

>Json.dump

```python
import json
file_path = './python_learn/my_json.json'
my_dict = {'name': 'lily', 'age': 18}
# 写入json
with open(file_path, 'w') as f:
    json.dump(my_dict, f)
```

## 读取json

```python
import json
file_path = './python_learn/my_json.json'
# 读取json 为了健壮性，这里应该有try-except
with open(file_path) as f:
    my_dict = json.load(f)
print(type(my_dict))
print(my_dict)
```

