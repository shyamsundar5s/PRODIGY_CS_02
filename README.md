from PIL import Image

def encrypt_image(image_path, output_path, key):
    # Open the image
    image = Image.open(image_path)
    pixels = image.load()

    # Get image dimensions
    width, height = image.size

    # Encrypt image by modifying the pixel values using the key
    for x in range(width):
        for y in range(height):
            r, g, b = pixels[x, y]
            # Add key to each pixel's R, G, B values and wrap them around 0-255
            pixels[x, y] = ((r + key) % 256, (g + key) % 256, (b + key) % 256)

    # Save the encrypted image
    image.save(output_path)
    print(f"Encrypted image saved as {output_path}")

def decrypt_image(image_path, output_path, key):
    # Open the encrypted image
    image = Image.open(image_path)
    pixels = image.load()

    # Get image dimensions
    width, height = image.size

    # Decrypt image by reversing the modification of pixel values using the key
    for x in range(width):
        for y in range(height):
            r, g, b = pixels[x, y]
            # Subtract key from each pixel's R, G, B values and wrap them around 0-255
            pixels[x, y] = ((r - key) % 256, (g - key) % 256, (b - key) % 256)

    # Save the decrypted image
    image.save(output_path)
    print(f"Decrypted image saved as {output_path}")

def main():
    while True:
        choice = input("Do you want to (e)ncrypt or (d)ecrypt? (e/d): ").lower()
        if choice in ['e', 'd']:
            image_path = input("Enter the input image path: ")
            output_path = input("Enter the output image path: ")
            key = int(input("Enter the key (a number between 1 and 255): "))
            
            if choice == 'e':
                encrypt_image(image_path, output_path, key)
            elif choice == 'd':
                decrypt_image(image_path, output_path, key)
        else:
            print("Invalid choice. Please enter 'e' for encryption or 'd' for decryption.")
        
        again = input("Do you want to process another image? (y/n): ").lower()
        if again != 'y':
            print("Goodbye!")
            break

if _name_ == "_main_":
    main()
