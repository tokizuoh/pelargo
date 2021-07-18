# pelargo
Move Google Spread Sheet data to trello card.  
  
## Docker
  
### Version

```bash
> docker --version
Docker version 19.03.12, build 48a66213fe

> docker-compose --version
docker-compose version 1.27.2, build 18f557f9
```
  
### Build
  
```bash
> docker-compose up --build -d
```
  
### Run
  
```bash
> docker-compose exec app go run main.go
```
  
## Usage
  
### Google Spread Sheet
  
```
1 done    A    Apple
2 yet     B    Banana
3 yet     A    Canyon
4 yet     A    Deixis
5 yet     C    Eeeeee
```
  
### 1. Get trello board ID
  
```go
member, err := trelloClient.GetMember("TRELLO_USERNAME", trello.Defaults())
if err != nil {
    log.Fatal(err)
}
boards, err := member.GetBoards(trello.Defaults())
if err != nil {
    log.Fatal(err)
}

for i, b := range boards {
    log.Println(i, b.Name, b.ID)
}
```
  
### 2. Get trello list ID
```go
board, err := trelloClient.GetBoard("TRELLO_BOARD_ID", trello.Defaults())
if err != nil {
    log.Fatal(err)
}

lists, err := board.GetLists(trello.Defaults())
if err != nil {
    log.Fatal(2)
}
for i, l := range lists {
    log.Println(i, l.ID, l.Name)
}
```
  
### 3. Set variables to .env
  
```bash
SPREADSHEET_ID=xxxxxx  # https://docs.google.com/spreadsheets/d/{HERE}/edit
READ_RANGE={SHEET_NAME}!{START_POSITION}:{END_POSITION}  # ex.) hoge!A1:E30
TRELLO_API_KEY=xxxxxx  # Get from https://trello.com/1/appKey/generate
TRELLO_API_TOKEN=xxxxxx  # Get it by following the produre of TRELLO_API_KEY
TRELLO_BOARD_ID=xxxxxx  # 1
TRELLO_LIST_ID=xxxxxx  # 2
```
  
### 4. Make the title you want to make a card
```go
if len(resp.Values) == 0 {
    fmt.Println("No data found.")
} else {
    fmt.Println("Name, Major:")
    for _, row := range resp.Values {
        isDone := row[1] == "done"
        if isDone {
            continue
        }

        // here
        title := row[3].(string)
        category := row[2].(string)
        cardTitle := fmt.Sprintf("[%v] %v", category, title)  // ex.) [B] Banana
        addCard(list, cardTitle)
    }
}
```

### 5. Run script
  
```bash
docker-compose exec app go run main.go
```

### 6. Registered as a card
Check trello.