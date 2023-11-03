# XML With Haskell

## Using the Text.XML package
- read an XML file into a Document object
    ```haskell
    Document prologue root epilogue <- readFile def input
    ```
    - root is the root node of the body of the xml document
    - use `elementNodes (root :: node)` to get a list of childrent nodes
- you can use pattern matching and recursive descent to parse them into objects, but it's easier to use Cursors and Axis
    - [cursors](https://hackage.haskell.org/package/xml-conduit-1.9.1.3/docs/Text-XML-Cursor.html)
    ```haskell
        let cursor = fromNode node
    ```
- you can get an element's attributes as map (Data.Map) with `elementAttributes`
    ```haskell
    getAttribute :: Node -> T.Text -> Maybe T.Text
    getAttribute (NodeElement elem) attrName = let attrMap = elementAttributes elem in
        M.lookup (quickName attrName) attrMap
    getAttribute _ _ = Nothing
    ```
    - the map's key is of the Name type, which has three fields. If the xml just has local names and doesn't use prefixes or namespaces, you can just use `nameLocalName` to pull the `T.Text` value out of the `Name` record
    ```haskell
    getName :: Node -> Maybe Name
    getName (NodeElement elem) = Just $ elementName elem
    getName _ = Nothing

    getLocalName :: Node -> Maybe T.Text
    getLocalName node = nameLocalName <$> getName node
    ```
- since elements may or may not exist in the children list of a node, using `mayMaybe` to run code only on the things that exists is nice
