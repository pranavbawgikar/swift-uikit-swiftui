# UITableView
`indexPath` represents the position in the table, a table can have sections and rows. There can be `n` sections and each section can have `n` rows within.
## UITableViewDelegate
`UITableViewDelegate` handles cell interactions.
## UITableViewDataSource
`UITableViewDataSource` needs minimum two functions: `cellForRowAt` and `numberOfRowsInSection`, everything else in the protocol is optional.
