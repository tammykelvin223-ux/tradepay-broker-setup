import React, { useEffect } from 'react';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { Badge } from '@/components/ui/badge';
import { 
  Shield, 
  Users, 
  TrendingUp, 
  DollarSign,
  AlertTriangle,
  Activity,
  Settings,
} from 'lucide-react';

interface User {
  role?: string;
  isAdmin?: boolean;
  email?: string;
  [key: string]: unknown;
}

export default function AdminPanel() {
  const currentUser: User = JSON.parse(localStorage.getItem('currentUser') || '{}');
  const isAdmin = currentUser.role === 'admin' || currentUser.isAdmin === true || currentUser.email?.includes('admin');

  useEffect(() => {
    // Redirect non-admin users
    if (!isAdmin) {
      window.location.href = '/';
    }
  }, [isAdmin]);

  // Don't render anything for non-admin users
  if (!isAdmin) {
    return (
      <div className="min-h-screen bg-gradient-to-br from-gray-900 via-blue-900 to-gray-900 flex items-center justify-center">
        <Card className="bg-gray-800 border-red-700">
          <CardContent className="p-8 text-center">
            <AlertTriangle className="w-16 h-16 text-red-500 mx-auto mb-4" />
            <h2 className="text-2xl font-bold text-white mb-2">Access Denied</h2>
            <p className="text-gray-400">You don't have permission to access this page.</p>
          </CardContent>
        </Card>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-gradient-to-br from-gray-900 via-blue-900 to-gray-900 p-4 lg:p-8">
      <div className="max-w-6xl mx-auto space-y-6">
        {/* Header */}
        <div className="flex items-center justify-between">
          <div>
            <h1 className="text-3xl font-bold text-white mb-2">Admin Panel</h1>
            <p className="text-blue-200">System administration and management</p>
          </div>
          <Badge className="bg-red-600 text-white px-4 py-2">
            <Shield className="w-4 h-4 mr-2" />
            Administrator Access
          </Badge>
        </div>

        {/* Admin Stats */}
        <div className="grid grid-cols-1 md:grid-cols-4 gap-6">
          <Card className="bg-gray-800 border-gray-700">
            <CardContent className="p-6">
              <div className="flex items-center space-x-4">
                <div className="w-12 h-12 bg-blue-600 rounded-lg flex items-center justify-center">
                  <Users className="w-6 h-6 text-white" />
                </div>
                <div>
                  <p className="text-gray-400 text-sm">Total Users</p>
                  <p className="text-2xl font-bold text-white">1,247</p>
                </div>
              </div>
            </CardContent>
          </Card>

          <Card className="bg-gray-800 border-gray-700">
            <CardContent className="p-6">
              <div className="flex items-center space-x-4">
                <div className="w-12 h-12 bg-green-600 rounded-lg flex items-center justify-center">
                  <TrendingUp className="w-6 h-6 text-white" />
                </div>
                <div>
                  <p className="text-gray-400 text-sm">Active Investments</p>
                  <p className="text-2xl font-bold text-white">₹45.2M</p>
                </div>
              </div>
            </CardContent>
          </Card>

          <Card className="bg-gray-800 border-gray-700">
            <CardContent className="p-6">
              <div className="flex items-center space-x-4">
                <div className="w-12 h-12 bg-purple-600 rounded-lg flex items-center justify-center">
                  <Activity className="w-6 h-6 text-white" />
                </div>
                <div>
                  <p className="text-gray-400 text-sm">Auto Trading Bots</p>
                  <p className="text-2xl font-bold text-white">89</p>
                </div>
              </div>
            </CardContent>
          </Card>

          <Card className="bg-gray-800 border-gray-700">
            <CardContent className="p-6">
              <div className="flex items-center space-x-4">
                <div className="w-12 h-12 bg-yellow-600 rounded-lg flex items-center justify-center">
                  <DollarSign className="w-6 h-6 text-white" />
                </div>
                <div>
                  <p className="text-gray-400 text-sm">Total Revenue</p>
                  <p className="text-2xl font-bold text-white">₹12.8M</p>
                </div>
              </div>
            </CardContent>
          </Card>
        </div>

        {/* Admin Functions */}
        <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
          <Card className="bg-gray-800 border-gray-700">
            <CardHeader>
              <CardTitle className="text-white flex items-center gap-2">
                <Users className="w-5 h-5" />
                User Management
              </CardTitle>
            </CardHeader>
            <CardContent className="space-y-4">
              <p className="text-gray-400">Manage user accounts, permissions, and verification status.</p>
              <div className="space-y-2">
                <div className="flex justify-between items-center p-3 bg-gray-700 rounded">
                  <span className="text-white">Pending KYC Verifications</span>
                  <Badge className="bg-yellow-600 text-white">23</Badge>
                </div>
                <div className="flex justify-between items-center p-3 bg-gray-700 rounded">
                  <span className="text-white">New Registrations (24h)</span>
                  <Badge className="bg-green-600 text-white">12</Badge>
                </div>
                <div className="flex justify-between items-center p-3 bg-gray-700 rounded">
                  <span className="text-white">Suspended Accounts</span>
                  <Badge className="bg-red-600 text-white">3</Badge>
                </div>
              </div>
            </CardContent>
          </Card>

          <Card className="bg-gray-800 border-gray-700">
            <CardHeader>
              <CardTitle className="text-white flex items-center gap-2">
                <Settings className="w-5 h-5" />
                System Settings
              </CardTitle>
            </CardHeader>
            <CardContent className="space-y-4">
              <p className="text-gray-400">Configure system-wide settings and parameters.</p>
              <div className="space-y-2">
                <div className="flex justify-between items-center p-3 bg-gray-700 rounded">
                  <span className="text-white">Trading Bot Status</span>
                  <Badge className="bg-green-600 text-white">Active</Badge>
                </div>
                <div className="flex justify-between items-center p-3 bg-gray-700 rounded">
                  <span className="text-white">System Health</span>
                  <Badge className="bg-green-600 text-white">Optimal</Badge>
                </div>
                <div className="flex justify-between items-center p-3 bg-gray-700 rounded">
                  <span className="text-white">Database Status</span>
                  <Badge className="bg-green-600 text-white">Online</Badge>
                </div>
              </div>
            </CardContent>
          </Card>
        </div>

        {/* Recent Activity */}
        <Card className="bg-gray-800 border-gray-700">
          <CardHeader>
            <CardTitle className="text-white flex items-center gap-2">
              <Activity className="w-5 h-5" />
              Recent Admin Activity
            </CardTitle>
          </CardHeader>
          <CardContent>
            <div className="space-y-3">
              <div className="flex items-center justify-between p-3 bg-gray-700 rounded">
                <div>
                  <p className="text-white font-medium">User verification approved</p>
                  <p className="text-gray-400 text-sm">john.doe@example.com - 2 minutes ago</p>
                </div>
                <Badge className="bg-green-600 text-white">Approved</Badge>
              </div>
              <div className="flex items-center justify-between p-3 bg-gray-700 rounded">
                <div>
                  <p className="text-white font-medium">Investment plan updated</p>
                  <p className="text-gray-400 text-sm">Gold Plan parameters modified - 15 minutes ago</p>
                </div>
                <Badge className="bg-blue-600 text-white">Updated</Badge>
              </div>
              <div className="flex items-center justify-between p-3 bg-gray-700 rounded">
                <div>
                  <p className="text-white font-medium">System maintenance completed</p>
                  <p className="text-gray-400 text-sm">Database optimization - 1 hour ago</p>
                </div>
                <Badge className="bg-purple-600 text-white">Completed</Badge>
              </div>
            </div>
          </CardContent>
        </Card>
      </div>
    </div>
  );
}
