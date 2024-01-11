        var collapsedList = inputList
            .GroupBy(item => item.ID)
            .Select(group =>
            {
                int sumSettledQty = group.Sum(item => item.Mode == "A" ? 0 : item.SettledQty);
                return new Item
                {
                    ID = group.Key,
                    Mode = "A",
                    SettledQty = sumSettledQty,
                    BorrowQty = sumSettledQty
                };
            })
            .ToList();
