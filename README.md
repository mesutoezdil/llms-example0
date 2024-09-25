# **Prediction Guard API: Exploring Capabilities with Go**

This project explores the capabilities of the Prediction Guard API using Go. It demonstrates how to interact with the API to generate text completions using various models, how to customize prompts to obtain different outputs, and discusses the flexibility of the API in producing creative or predictable results.

## **Project Overview**

This Go project connects with the Prediction Guard API to generate text completions based on prompts provided by the user. We explore how to:
- Make API requests.
- Retrieve supported models.
- Customize prompts to generate diverse responses.
- Adjust parameters like `temperature` to control the output's creativity.

## **API Integration**

The main integration flow includes:
1. Setting up the Prediction Guard API key.
2. Using Go to send POST requests to the API's `/completions` endpoint.
3. Experimenting with different models, prompts, and parameters.

### **Key Code Snippet**

Here’s the main code used to interact with the API:

```go
package main

import (
	"context"
	"fmt"
	"log"
	"os"
	"time"

	"github.com/predictionguard/go-client"
)

func main() {
	if err := run(); err != nil {
		log.Fatalln(err)
	}
}

func run() error {
	host := "https://api.predictionguard.com"
	apiKey := os.Getenv("PGKEY")

	logger := func(ctx context.Context, msg string, v ...any) {
		s := fmt.Sprintf("msg: %s", msg)
		for i := 0; len(v) > i; i += 2 {
			s = s + fmt.Sprintf(", %s: %v", v[i], v[i+1])
		}
		log.Println(s)
	}

	cln := client.New(logger, host, apiKey)

	ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
	defer cancel()

	Float32Ptr := func(f float32) *float32 {
		return &f
	}

	input := client.CompletionInput{
		Model:       "Hermes-2-Pro-Mistral-7B",  // Selected model
		Prompt:      "The best thing about gophers is: ",
		MaxTokens:   100,
		Temperature: Float32Ptr(0.75),
	}

	resp, err := cln.Completions(ctx, input)
	if err != nil {
		return fmt.Errorf("ERROR: %w", err)
	}

	fmt.Println(resp.Choices[0].Text)
	return nil
}
```

## **How the Prompt Works**

The **prompt** defines what the API should generate a response about. In this example:

```go
Prompt: "The best thing about gophers is: ",
```

This text acts as a starting point for the model to generate content. Changing this prompt will result in different output. Here’s an example of the output generated from the above prompt.

### **Example 1: Original Prompt**

- **Prompt**: `"The best thing about gophers is: "`
- **Output**:

   ```
   they don’t care what the neighbors think.  I love my gophers.  I love the way they munch their way through my lawn, oblivious to the world around them.  I love the way their little faces peek out from their burrows, all pink and innocent.  I love the way they scamper across the yard, their tiny paws scooping up clumps of dirt.  I love them so much, in fact, that I
   ```

### **Example 2: Different Prompt**

Changing the prompt leads to different results. For example, if we change the prompt to:

```go
Prompt: "Tell me about the history of computers: "
```

- **Output**:

   ```
   The history of computers spans over centuries, starting with simple mechanical devices to modern digital computers. The first known computing device, the abacus, was invented in ancient Mesopotamia. Over time, various inventors contributed to computing, including Charles Babbage with his concept of the Analytical Engine, and Alan Turing, who laid the groundwork for modern computing theory. Today, computers are an integral part of society.
   ```

### **Experimenting with `Temperature`**

The `temperature` parameter controls the randomness of the output. A lower temperature (e.g., `0.2`) makes the responses more deterministic, while a higher temperature (e.g., `1.0`) encourages more diverse, creative responses.

For example:
- **Temperature = 0.2**: Results in more predictable, repetitive output.
- **Temperature = 1.0**: Allows for more varied and creative responses.

## **Capabilities and Customization**

The Prediction Guard API offers the following key capabilities:
- **Custom Prompts**: You can ask the model to complete or continue any kind of sentence, prompt, or question.
- **Model Selection**: Use different models depending on your needs. Models vary in size and power, with larger models typically offering more nuanced responses.
- **Parameter Tuning**: Adjust parameters like `MaxTokens` to limit the length of the response and `Temperature` to control creativity.

## **How to Run This Project**

### 1. **Clone the Repository**

```bash
git clone https://github.com/mesutoezdil/llms-example0.git
```

### 2. **Set Your API Key**

Retrieve your API key from Prediction Guard and set it in your environment:

```bash
export PGKEY="your_actual_api_key"
```

### 3. **Run the Code**

Run the Go program with the following command:

```bash
go run main.go
```

This will output a response based on the prompt you provided.

## **Conclusion**

This project demonstrates how to integrate with the Prediction Guard API to generate text completions using Go. By customizing prompts and parameters, you can produce varied outputs and explore the potential of the models available through the API. The flexibility of the API allows for a wide range of applications, from content generation to answering questions.
