# Commands
```ShellOs
		/---------------Комманды-------------\
		| rmdir <<Название папки>>           |
		| tree                               |
		| clear                              |
		| help         [ShellOs]             |
		| exit                               |
		| dirs                               |
		| cd <<Название>> или cd - or cd ~   |
		| mkdir <<Название>>                 |
		| whoami                             |
		| say <<Текст>>                      |
		| file <<Название>> <<Текст>>        |
		| read <<Название>>                  |
		| files                              |
		\------------------------------------/
```
# Code
```Python
from os import getlogin, system, name as _name
from random import randint

dirs =         {'shellos': {'PATH': f'C:\\Users\\{getlogin()}'}}
history =      []
path =         f"C:\\Users\\{getlogin()}"
home_path =    path
files =        {'license': {'PATH': 'C:\\Users\\Iskander\\shellos', 'CONTENT': '''
Copyright 2025 Nskander

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
	'''}}

def mkdir(dirname):
    dirs[dirname]={"PATH": path}
    print(f"Создана директория '{dirname}' в '{path}'")

def move(filename, _path=path):
	if filename in files:
		filename["PATH"]=_path

def file(filename, content):
	files[filename]={"PATH": path, "CONTENT": content}
	print(f"Создан файл '{filename}' в '{path}'")
def read(filename):
	if filename in files and files[filename]['PATH']==path:
		print(files[filename]['CONTENT'])


def cd(dirname):
    global path
    if dirname == "-":
        parts = path.split("\\")
        if len(parts) > 1:
            path = "\\".join(parts[:-1])
    elif dirname == "~":
        if path == home_path:
            print("Вы уже находитесь в домашней директории")
        else:
            path = home_path
    elif dirname in dirs:
        if dirs[dirname]["PATH"] == path:
            path += "\\" + dirname
def rmdir(dirname):
	if dirname in dirs and dirs[dirname]["PATH"]==path:
		del dirs[dirname]
def tree():
	print(dirs)

def run(command):
	add = True
	c = command.lower()
	if c.startswith("mkdir "):
		dirname = c[len('mkdir '):].strip()
		mkdir(dirname)
	elif c.startswith("cd "):
		dirname = c[len('cd '):].strip()
		cd(dirname)
	elif c == "dirs":
		print(dirs)
	elif c == "exit":
		exit()
	elif c.startswith("rmdir "):
		folder = c[len("rmdir "):].strip()
		rmdir(folder)
	elif c == 'tree':
		tree()
	elif c.startswith("say "):
		text = c[len('say '):].strip()
		print(text)
	elif c.startswith("whoami"):
		print(getlogin())
	elif c == "clear":
		system("cls" if _name.lower() == "nt" else "clear")
	elif c == "help":
		helper = r"""
		/---------------Комманды-------------\
		| rmdir <<Название>>                 |
		| rmfile <<Название>>                |
		| tree                               |
		| clear                              |
		| help         [ShellOs]             |
		| exit                               |
		| dirs                               |
		| cd <<Название>> или cd - or cd ~   |
		| mkdir <<Название>>                 |
		| whoami                             |
		| say <<Текст>>                      |
		| file <<Название>> <<Текст>>        |
		| read <<Название>>                  |
		| files                              |
		| touch <<Название>>                 |
		| history                            |
		| rename <<Прошлое>> <<После>>       |
		| math <<Пример>>                    |
		| rand <<Минимум>> <<Максимум>>      |
		\------------------------------------/
		"""
		print(helper)
	elif c.startswith("file "):
		_file = c.split()
		file(_file[1], _file[2])
	elif c.startswith("read "):
		_file_name = c[len('read '):].strip()
		read(_file_name)
	elif c == "files": print(files)
	elif c.startswith('math'):
		vars_ = c[len('math '):].strip().split()
		num1 = float(vars_[0])
		num2 = float(vars_[2])
		opr = vars_[1]
		if opr == "+": print(num1 + num2)
		if opr == "-": print(num1 - num2)
		if opr == "*": print(num1 * num2)
		if opr == "/": print(round(num1 / num2, 5) if int(num2) != 0 else "Ошибка: Деление на ноль")
		if opr == "^" or opr == "**": print(num1 ** num2)
		if opr == "%": print(num1 % num2)
	elif c == "license": print("""

Copyright 2025 Nskander

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

	""")
	elif c == "pwd": print(path)
	elif c.startswith("touch "):
		filename = c[len("touch "):].strip()
		file(filename, "")
	elif c == 'history':
		print("\n".join(history))
	elif c.startswith("rename "):
		names = c.split()
		before = names[1]
		after = names[2]
		if before in files and files[before]["PATH"]==path:
			files[after]=files[before].copy()
			del files[before]
	elif c.startswith("copy "):
		_files = c.split()
		file_1 = _files[1]
		file_2 = _files[2]
		if before in files and files[before]["PATH"]==path:
			files[after]=files[before].copy()
	elif c.startswith('rmfile'):
		_filename = c[len("rmfile "):].strip()
		if _filename in files:
			del files[_filename]
	elif c.startswith('rand '):
		nums = c[len("rand "):].strip().split()
		mini = nums[0]
		maxi = nums[1]
		print(randint(int(mini), int(maxi)))
	elif c == "": pass
	else:
		add = False
		print(f"`{c}` не является командой")
	if add:
		if len(history) > 4:
			del history[0]
		history.append(c)
	add = True

if __name__ == "__main__":
	print(f"Привет {getlogin()}! ShellOs: Введите 'help' для существующих команд")
	while True:
		command = input(f"{path}> ")
		run(command)

```

# License
```license

Copyright 2025 Nskander

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

```
