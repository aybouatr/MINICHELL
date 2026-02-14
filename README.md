# 🐚 MINICHELL

A lightweight UNIX shell implementation in C, replicating the core functionality of bash.

## 📋 Table of Contents

- [About](#about)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Built-in Commands](#built-in-commands)
- [Project Structure](#project-structure)
- [Technical Details](#technical-details)
- [Contributors](#contributors)

## 🎯 About

MINICHELL is a 42 school project that implements a simple shell (command-line interpreter) similar to bash. The shell reads and executes commands, manages environment variables, handles redirections and pipes, and implements several built-in commands.

## ✨ Features

- **Interactive Command Prompt**: Displays `-> minishell` prompt for user input
- **Command History**: Navigate through previously entered commands using arrow keys
- **Pipes**: Chain multiple commands with `|` operator
- **Input/Output Redirections**:
  - `<` : Redirect input
  - `>` : Redirect output (overwrite)
  - `>>` : Redirect output (append)
  - `<<` : Here-document
- **Environment Variable Expansion**: Automatically expands `$VAR` variables
- **Quote Handling**: Supports single (`'`) and double (`"`) quotes
- **Wildcard Expansion**: Pattern matching with `*` (bonus feature)
- **Signal Handling**: 
  - `Ctrl-C`: Interrupts current command
  - `Ctrl-D`: Exits the shell
  - `Ctrl-\`: Ignored (SIGQUIT)
- **Exit Status**: Tracks and manages command exit statuses

## 🔧 Installation

### Prerequisites

- GCC compiler
- Make
- GNU Readline library

```bash
# Install readline library (Debian/Ubuntu)
sudo apt-get install libreadline-dev

# Install readline library (macOS)
brew install readline
```

### Build

```bash
# Clone the repository
git clone https://github.com/aybouatr/MINICHELL.git
cd MINICHELL

# Compile the project
make

# Run the shell
./minishell
```

### Makefile Commands

- `make` : Compile the project
- `make clean` : Remove object files
- `make fclean` : Remove object files and executable
- `make re` : Recompile the project

## 🚀 Usage

```bash
$ ./minishell
-> minishell ls -la
-> minishell echo "Hello, World!"
-> minishell cat file.txt | grep "pattern" | wc -l
-> minishell export PATH="/usr/bin:$PATH"
-> minishell cd /home/user
-> minishell exit
```

## 📦 Built-in Commands

| Command | Description | Usage |
|---------|-------------|-------|
| `echo` | Display a line of text | `echo [-n] [string ...]` |
| `cd` | Change directory | `cd [directory]` |
| `pwd` | Print working directory | `pwd` |
| `export` | Set environment variables | `export [name[=value] ...]` |
| `unset` | Unset environment variables | `unset [name ...]` |
| `env` | Display environment variables | `env` |
| `exit` | Exit the shell | `exit [n]` |

### Command Details

#### `echo`
```bash
-> minishell echo Hello World
Hello World
-> minishell echo -n "No newline"
No newline-> minishell
```

#### `cd`
```bash
-> minishell cd /tmp        # Change to /tmp
-> minishell cd             # Change to HOME directory
-> minishell cd -           # Change to previous directory
-> minishell cd .           # Stay in current directory
```

#### `export`
```bash
-> minishell export USER=john
-> minishell export PATH=$PATH:/new/path
-> minishell export VAR+=value  # Append to existing variable
-> minishell export            # Display all exported variables
```

#### `unset`
```bash
-> minishell unset USER
-> minishell unset VAR1 VAR2 VAR3
```

#### `exit`
```bash
-> minishell exit           # Exit with last command status
-> minishell exit 42        # Exit with status 42
-> minishell exit abc       # Error: numeric argument required
```

## 📁 Project Structure

```
MINICHELL/
├── builtins/              # Built-in command implementations
│   ├── re_cd.c           # cd command
│   ├── cd_utils.c        # cd helper functions
│   ├── re_echo.c         # echo command
│   ├── re_env.c          # env command
│   ├── re_exit.c         # exit command
│   ├── re_export.c       # export command
│   ├── export_utils.c    # export helper functions
│   ├── export_utils2.c   # additional export helpers
│   ├── re_pwd.c          # pwd command
│   └── re_unset.c        # unset command
├── execution/            # Command execution logic
├── parsing/              # Input parsing and tokenization
├── ft_here_doc/          # Here-document implementation
├── wildcards_bns/        # Wildcard expansion (bonus)
├── libft/                # Custom C library
├── minishell.h           # Main header file
├── main.c                # Entry point
└── makefile              # Build configuration
```

## 🔍 Technical Details

### Data Structures

#### Token Types
- `e_pipe` : Pipe operator
- `e_redir_in` : Input redirection `<`
- `e_redir_ou` : Output redirection `>`
- `e_redir_app` : Append redirection `>>`
- `e_here_doc` : Here-document `<<`
- `e_cmd` : Command
- `e_arg` : Command argument
- `e_builtins` : Built-in command

#### Global Data Structure
```c
typedef struct s_data {
    char    **env;           // Environment variables
    t_list  *save_head_gc;   // Garbage collector
    char    *cwd;            // Current working directory
    char    *oldpwd;         // Previous working directory
    int     exit_status;     // Last command exit status
    int     pipe;            // Pipe flag
    int     sig_here_doc;    // Here-doc signal flag
} t_data;
```

### Memory Management

The project implements a custom garbage collector to prevent memory leaks:
- `allocation()` : Allocates memory and tracks it
- `clean_memory()` : Frees all tracked memory
- `gc_free_one()` : Frees a specific pointer

### Signal Handling

```c
- SIGINT (Ctrl-C)  : Displays new prompt
- SIGQUIT (Ctrl-\) : Ignored
- EOF (Ctrl-D)     : Exits the shell
```

## 👥 Contributors

- [@aybouatr](https://github.com/aybouatr)
- [@oachbani](https://github.com/oachbani)

## 📝 Notes

This project is part of the 42 school curriculum. It was developed following the strict 42 Norm coding standards.

### Exit Status Codes

- `0` : Success
- `1` : General error
- `2` : Misuse of shell command
- `127` : Command not found
- `130` : Script terminated by Control-C

## 🐛 Known Limitations

- Does not support command substitution `$()`
- Does not support arithmetic expansion `$(())`
- Does not support logical operators `&&`, `||`
- No support for backslash escaping outside quotes

---

**Made with ❤️ at 1337 School (42 Network)**
