class File:
    def __init__(self, name):
        self.name = name
        self.content = ""
        self.permissions = {}

class Directory:
    def __init__(self, name):
        self.name = name
        self.contents = {}
        self.permissions = {}

class FileSystem:
    def __init__(self):
        self.root = Directory("/")
        self.current_directory = self.root
    
    def create_file(self, name):
        if name in self.current_directory.contents:
            print("File already exists.")
            return
        new_file = File(name)
        self.current_directory.contents[name] = new_file
    
    def delete_file(self, name):
        if name in self.current_directory.contents and isinstance(self.current_directory.contents[name], File):
            del self.current_directory.contents[name]
        else:
            print("File not found.")
    
    def read_file(self, name):
        if name in self.current_directory.contents and isinstance(self.current_directory.contents[name], File):
            return self.current_directory.contents[name].content
        else:
            return "File not found."
    
    def write_file(self, name, content):
        if name in self.current_directory.contents and isinstance(self.current_directory.contents[name], File):
            self.current_directory.contents[name].content = content
        else:
            print("File not found.")
    
    def create_directory(self, name):
        if name in self.current_directory.contents:
            print("Directory already exists.")
            return
        new_directory = Directory(name)
        self.current_directory.contents[name] = new_directory
    
    def change_directory(self, name):
        if name in self.current_directory.contents and isinstance(self.current_directory.contents[name], Directory):
            self.current_directory = self.current_directory.contents[name]
        else:
            print("Directory not found.")
    
    def list_contents(self):
        for name, item in self.current_directory.contents.items():
            if isinstance(item, File):
                print(f"File: {name}")
            elif isinstance(item, Directory):
                print(f"Directory: {name}")
    
    def set_permissions(self, name, permissions):
        if name in self.current_directory.contents:
            self.current_directory.contents[name].permissions = permissions
        else:
            print("Item not found.")
    
    def get_permissions(self, name):
        if name in self.current_directory.contents:
            return self.current_directory.contents[name].permissions
        else:
            return "Item not found."
        
fs = FileSystem()

fs.create_file("file1.txt")
fs.write_file("file1.txt", "Hello, World!")
print(fs.read_file("file1.txt"))

fs.create_directory("folder1")
fs.change_directory("folder1")
fs.create_file("file2.txt")
fs.write_file("file2.txt", "This is a new file.")

fs.list_contents()
