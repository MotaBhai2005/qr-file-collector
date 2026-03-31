# QR File Collector

A modern web application that enables instant file sharing and collection through QR codes. Simply generate a QR code, share it, and collect files from multiple sources in real-time.

## Features

✨ **QR Code Generation** - Generate unique QR codes for each file collection session
📱 **Mobile Friendly** - Optimized for both desktop and mobile devices
🔒 **Security** - Built-in file type filtering to prevent malicious uploads
📦 **File Management** - Support for multiple file uploads with size restrictions (50MB per file)
⚡ **Real-time Updates** - Live file list updates using Socket.IO
🌐 **Cross-Platform** - Works on local networks and cloud deployments
🧹 **Auto Cleanup** - Automatic cleanup of old files and expired sessions
🎨 **Modern UI** - Beautiful glassmorphism design with smooth animations

## Tech Stack

- **Backend**: Node.js with Express.js
- **File Upload**: Multer
- **QR Code Generation**: qrcode
- **Real-time Communication**: Socket.IO
- **Session Management**: UUID
- **Frontend**: HTML, CSS with modern animations

## Prerequisites

- Node.js (v14 or higher)
- npm

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd qr-file-collector
```

2. Install dependencies:
```bash
npm install
```

3. Start the server:
```bash
node server.js
```

The server will start on port 3000 (or the PORT environment variable if set):
```
🚀 Server running on port 3000
🌐 LAN IP: 192.168.x.x:3000
```

## Usage

### For File Collectors (Host)

1. Navigate to `http://localhost:3000` in your browser
2. Click "Generate QR Code" to create a new upload session
3. A unique QR code will be displayed along with the upload URL
4. Share the QR code or URL with others
5. Watch files appear in real-time as people upload them
6. Download or manage uploaded files

### For File Contributors (Uploaders)

1. Scan the QR code with your mobile device or click the upload link
2. You'll be taken to the upload page
3. Select files to upload (drag & drop or click to browse)
4. Files are instantly uploaded and visible to the collector
5. Maximum file size is 50MB per file

## Configuration

### Environment Variables

- `PORT` - Server port (default: 3000)

### Security Settings

**Blocked File Types** (to prevent malware):
- `.exe` - Executable files
- `.bat` - Batch scripts
- `.sh` - Shell scripts
- `.cmd` - Command scripts
- `.msi` - Windows installers
- `.vbs` - VBScript files
- `.docm` - Macro-enabled documents

**File Size Limits**:
- Maximum file size: 50MB per upload

### Auto-Cleanup Settings

Files and sessions are automatically cleaned according to these rules:
- **Uploaded Files**: Deleted after 24 hours
- **Sessions**: Removed from memory after 12 hours
- **Cleanup Interval**: Every 1 hour

## Project Structure

```
qr-file-collector/
├── server.js              # Main Express server
├── package.json           # Dependencies and metadata
├── public/
│   ├── index.html         # Main QR generation page
│   └── upload.html        # File upload page
├── uploads/               # Directory for uploaded files
└── README.md             # This file
```

## API Endpoints

### `GET /`
Serves the main page for generating QR codes.

### `GET /generate`
Generates a new upload session with a QR code.

**Response:**
```json
{
  "id": "uuid-session-id",
  "qr": "data:image/png;base64,...",
  "uploadUrl": "http://192.168.x.x:3000/qr/uuid-session-id",
  "isLocal": true
}
```

### `GET /qr/:id`
Redirects to the upload page for the given session.

### `GET /upload/:id`
Serves the upload page for a specific session.

### `POST /upload/:id`
Handles file uploads for a session. Accepts multipart form data with files.

**Response:**
```json
{
  "success": true
}
```

## Real-time Events (Socket.IO)

### Client Events

- **join** - Emit when connecting to a session
  ```javascript
  socket.emit('join', sessionId);
  ```

### Server Events

- **filesUpdated** - Broadcast when files are uploaded
  ```javascript
  {
    "files": [...],
    "total": 5
  }
  ```

## Error Handling

The application handles various error scenarios:

- **Invalid File Type**: Returns 400 error with message "File type not allowed! (Possible Malware)"
- **File Size Exceeded**: Returns 400 error with message "File size exceeds 50MB limit!"
- **Upload Failure**: Returns 500 error with message "Upload failed."

## Deployment

### Local Network
The application automatically detects and uses your local network IP address for QR codes generated on the local network.

### Cloud Deployment
The app supports deployment to cloud platforms including:
- Render
- Vercel
- ngrok (loca.lt)
- Cloudflare Tunnel (trycloudflare.com)

When deployed to these platforms, HTTPS URLs are used for QR code generation.

## Performance Considerations

- **Memory**: Sessions are stored in-memory and automatically cleaned up
- **Disk Space**: Old files are automatically deleted after 24 hours
- **Concurrent Users**: The app uses Socket.IO for efficient real-time communication

## Troubleshooting

### QR Code not showing?
- Check that your server is running and accessible
- Ensure your firewall isn't blocking port 3000
- Try accessing `http://localhost:3000` directly

### Files not uploading?
- Check that the file size doesn't exceed 50MB
- Verify the file type is allowed (not in blocked list)
- Ensure the uploads directory exists (created automatically)

### Can't access from another device?
- Use the LAN IP address displayed when the server starts (e.g., `http://192.168.x.x:3000`)
- Make sure both devices are on the same network
- Check firewall settings

## License

ISC

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Security Notes

- Files are stored with timestamps in their filenames to prevent overwriting
- Malicious file types are blocked to prevent code execution
- Sessions expire after 12 hours for memory management
- Files are automatically deleted after 24 hours

## Future Enhancements

Possible features for future versions:
- User authentication and password protection for sessions
- Compression of uploaded files
- Preview functionality for various file types
- Downloadable archives of all files
- Activity logging and analytics
