# Wikipedia Article Parsing Functionalities


## To parse a Wikipedia article  (in XML format), you can use the following code snippet. 

```
WikipediaEntity entity = new WikipediaEntity();
entity.setTitle(entity_name); //set the title of the Wikipedia article
entity.setMainSectionsOnly(true); //determine whether you wanna extract only main sections, or the nested sections
entity.setSplitSections(true); //determine if you are interested in parsing and splitting the Wikipedia content article into sections
entity.setCleanReferences(true); //flag if you wanna remove the [File] references within the article
entity.setExtractReferences(false); //flag if you want to extract and parse the references from the article
entity.setContent(entity_text); //set the actual XML content of the article
```


## Output all the extracted citations for a Wikipedia article.

```
Map<Integer, Map<String, String>> citations = entity.getEntityCitations();
Map<Integer, Map<String, List<String>>> citing_statements = entity.getCitingStatements();

StringBuffer sb = new StringBuffer();
for (int cite_id : citing_statements.keySet()) {
    Map<String, String> citation = citations.get(cite_id);
    String type = citation.containsKey("type") ? citation.get("type") : "N/A";
    String url = citation.containsKey("url") ? citation.get("url") : "N/A";
    for (String section : citing_statements.get(cite_id).keySet()) {
        for (String citing_statement : citing_statements.get(cite_id).get(section)) {
            String statement = citing_statement.replaceAll("\t+", " ").trim();
            sb.append(entity.getTitle()).append("\t").append(section).append("\t");
            sb.append(cite_id).append("\t").append(type).append("\t");
            sb.append(url).append("\t").append(statement).append("\n");
        }
    }
}
```
