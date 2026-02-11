```C#
Random rand = new Random();
for (int i = 0; i < row; ++i)
{
    for (int j = 0; j < col; ++j)
    {
        matrix[i, j] = rand.Next(start, end);
    }
}
```